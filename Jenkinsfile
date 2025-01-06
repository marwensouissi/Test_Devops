pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'docker-log'  // Replace with your Docker credentials ID
        FRONTEND_IMAGE = 'marwensouissi/angulardevops'
        BACKEND_IMAGE = 'marwensouissi/springboot'
        KUBECONFIG = 'C:\\path\\to\\.kube\\config' // Replace with the correct kubeconfig path for Windows
    }



    stages {
        

        
        stage('Checkout Code') {
            steps {
                echo 'Cloning GitHub repository...'
                git branch: 'master', url: 'https://github.com/marwensouissi/Test_Devops.git'
            }
        }
        
        
                stage('Checkout Code') {
            steps {
                echo 'Cloning GitHub repository...'
                git branch: 'master', url: 'https://github.com/marwensouissi/Test_Devops.git'
            }
        }
        
        
        stage('Login to Docker') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        powershell '''
                        docker login -u $env:DOCKER_USERNAME -p $env:DOCKER_PASSWORD
                        '''
                    }
                }
            }
        }



        stage('Build Frontend') {
            steps {
                echo 'Building Angular frontend...'
                powershell '''
                cd Angular_Gestion_Foyer
                npm install --force
                npm run build --prod
                '''
            }
        }

        stage('Build Backend') {
            steps {
                echo 'Building Spring Boot backend...'
                powershell '''
                $env:JAVA_HOME = "C:\\Program Files\\Java\\jdk-17" # Update with the correct Java path
                $env:Path += ";$env:JAVA_HOME\\bin"
                cd myFirstProject
                .\\mvnw clean package -DskipTests
                '''
            }
        }

        stage('Build and Push Docker Images') {
            steps {
                parallel (
                    frontend: {
                        script {
                            withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                                powershell '''
                                echo "Building and pushing frontend image..."
                                cd Angular_Gestion_Foyer
                                docker build -t marwensouissi/angulardevops:latest .
                                echo $env:DOCKER_PASSWORD | docker login -u $env:DOCKER_USERNAME --password-stdin
                                docker push marwensouissi/angulardevops:latest
                                '''
                            }
                        }
                    },
                    backend: {
                        script {
                            withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                                powershell '''
                                echo "Building and pushing backend image..."
                                cd myFirstProject
                                docker build -t marwensouissi/springboot:latest .
                                echo $env:DOCKER_PASSWORD | docker login -u $env:DOCKER_USERNAME --password-stdin
                                docker push marwensouissi/springboot:latest
                                '''
                            }
                        }
                    }
                )
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo 'Applying Kubernetes manifests...'
                powershell '''
        kubectl apply -f k8s/mysql.yml --validate=false
        kubectl apply -f k8s/backend-deployment.yml --validate=false
        kubectl apply -f k8s/frontend-deployment.yml --validate=false
        kubectl apply -f k8s/configmap.yaml --validate=false
                '''
            }
        }

        stage('Port Forward Services') {
            steps {
                echo 'Setting up port forwarding for testing...'
                script {
                    powershell 'Start-Process -NoNewWindow -FilePath "kubectl" -ArgumentList "port-forward svc/backend 8089:8089"'
                    powershell 'Start-Process -NoNewWindow -FilePath "kubectl" -ArgumentList "port-forward svc/frontend 4200:4200"'
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up port-forward processes...'
            powershell '''
            Stop-Process -Name kubectl -Force -ErrorAction SilentlyContinue
            '''
        }
    }
}