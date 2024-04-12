pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
//      SONARQUBE_SERVER_URL = 'http://sonarqube-server:9000' // Update with your SonarQube server URL
//      SONARQUBE_TOKEN = credentials('sonarqube-token') // Jenkins credentials for SonarQube token
    }

    stages {
        stage('Pull Docker Image') {
            steps {
                script {
                    // Log in to Docker Hub using credentials
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                        sh "docker login -u $DOCKER_HUB_USERNAME -p $DOCKER_HUB_PASSWORD"
                    }

                    // Pull the existing Docker image from Docker Hub
                    sh 'docker pull purema4/pinball:latest'
                }
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Running..."'
            }
        }

/*    stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube analysis using SonarScanner CLI
                    withSonarQubeEnv('SonarQube') {
                        sh "sonar-scanner -Dsonar.host.url=${SONARQUBE_SERVER_URL} -Dsonar.login=${SONARQUBE_TOKEN}"
                    }
                }
            }
        }
*/
        stage('Deploy to Kubernetes') {
            steps {
                // Deploy to Kubernetes cluster
                sh 'kubectl apply -f devops_personal/deployment.yaml'

                // Check deployment status
                sh 'kubectl get pods'
            }
        }
    }

    post {
        always {
            // Clean up Docker images after deployment
            sh 'docker logout'
        }
    }
}
