pipeline {
    agent any
    
    tools {
        nodejs 'nodejs20'
    }
    
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        IMAGE_TAG    = "${env.BUILD_NUMBER}"
        DOCKER_USER  = "majid04"
    }

    stages {
        stage('1. git checkout') {
            steps {
                git branch: 'Majidullask04-patch-1', url: 'https://github.com/Majidullask04/microservices-demo.git'
            }
        }
        
        stage('2. Discover Repository') {
            steps {
                sh '''
                echo "=== Project File Structure ==="
                find . -maxdepth 3 \( -name "package.json" -o -name "pom.xml" -o -name "build.gradle" -o -name "go.mod" -o -name "*.csproj" -o -name "requirements.txt" -o -name "Dockerfile" -o -name "docker-compose.yml" \)
                '''
            }
        }

        stage('3. gitleaks') {
            steps {
                sh 'gitleaks detect --source=. --verbose || true'
            }
        }
        
        stage('4. sonar-scanner') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh '''
                    $SCANNER_HOME/bin/sonar-scanner \
                    -Dsonar.projectName=microservice-cloud-demo \
                    -Dsonar.projectKey=microservice-cloud-demo \
                    -Dsonar.exclusions=**/*.java
                    '''
                }
            }
        }
        
        stage('5. Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: false
                }
            }
        }

        stage('6. Trivy FS Scan') {
            steps {
                sh 'trivy fs --severity HIGH,CRITICAL .'
            }
        }
        
        stage('7. build & push docker') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-id') {
                        sh '''
                        FAILED=0
                        for dockerfile in $(find src -name Dockerfile); do
                            service=$(echo "$dockerfile" | cut -d'/' -f2)
                            echo "=================================================="
                            echo "Building $service from $dockerfile..."
                            echo "=================================================="

                            if docker build \
                                -t ${DOCKER_USER}/${service}:${IMAGE_TAG} \
                                -t ${DOCKER_USER}/${service}:latest \
                                -f "$dockerfile" \
                                "$(dirname "$dockerfile")"; then
                                
                                echo "Build successful for $service. Pushing image..."
                                docker push ${DOCKER_USER}/${service}:${IMAGE_TAG}
                                docker push ${DOCKER_USER}/${service}:latest
                            else
                                echo "ERROR: Build failed for $service"
                                FAILED=1
                            fi
                        done

                        if [ $FAILED -ne 0 ]; then
                            echo "ERROR: One or more microservice builds failed!"
                            exit 1
                        fi
                        '''
                    }
                }
            }
        }
        
        stage('8. Deploy to k3s') {
            steps {
                sh '''
                # Ensure the target namespace exists in k3s
                kubectl create namespace microservices --dry-run=client -o yaml | kubectl apply -f -

                # Update kustomize images to point to built tags if kustomize is available
                if command -v kustomize &> /dev/null; then
                    cd kubernetes-manifests
                    for dockerfile in $(find ../src -name Dockerfile); do
                        service=$(echo "$dockerfile" | cut -d'/' -f2)
                        kustomize edit set image $service=${DOCKER_USER}/${service}:${IMAGE_TAG} || true
                    done
                    cd ..
                fi

                # Apply manifests to k3s cluster
                kubectl apply -n microservices -k kubernetes-manifests
                '''
            }
        }
    }

    post {
        always {
            echo "Pipeline execution finished."
        }
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check stage logs for details."
        }
    }
}
