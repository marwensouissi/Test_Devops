pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'  // Replace with your Docker credentials ID
        FRONTEND_IMAGE = 'marwensouissi/angulardevops'
        BACKEND_IMAGE = 'marwensouissi/springboot'
        KUBECONFIG = '/var/lib/jenkins/.kube/config' // Path to kubeconfig file

    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Cloning GitHub repository...'
                git branch: 'master', url: 'https://github.com/marwensouissi/Test_Devops.git'
            }
        }

        stage('Build Frontend') {
            steps {
                echo 'Building Angular frontend...'
                sh '''
                cd Angular_Gestion_Foyer
                npm install --force
                npm run build --prod
                '''
            }
        }

        stage('Build Backend') {
            steps {
                echo 'Building Spring Boot backend...'
                sh '''
                cd myFirstProject
                chmod +x ./mvnw 
                ./mvnw clean package -DskipTests
                '''
            }
        }

stage('Build and Push Docker Images') {
    steps {
        parallel (
            frontend: {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh '''
                        echo "Building and pushing frontend image..."
                        cd Angular_Gestion_Foyer
                        docker build -t $FRONTEND_IMAGE:latest .
                        docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
                        docker push $FRONTEND_IMAGE:latest
                        '''
                    }
                }
            },
            backend: {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh '''
                        echo "Building and pushing backend image..."
                        cd myFirstProject
                        docker build -t $BACKEND_IMAGE:latest .
                        docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
                        docker push $BACKEND_IMAGE:latest
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
                sh '''
                kubectl apply -f k8s/mysql.yml
                kubectl apply -f k8s/backend-deployment.yml
                kubectl apply -f k8s/frontend-deployment.yml
                kubectl apply -f k8s/configmap.yaml
                
                '''
            }
        }

        stage('Port Forward Services') {
            steps {
                echo 'Setting up port forwarding for testing...'
                script {
                    // Port-forward backend service
                    sh 'kubectl port-forward svc/backend 8089:8089 &'
                    
                    // Port-forward frontend service
                    sh 'kubectl port-forward svc/frontend 4200:4200 &'
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up port-forward processes...'
            sh 'pkill -f "kubectl port-forward" || true'
        }
    }
}
