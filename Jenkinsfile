pipeline {
    agent any

    stages{
        stage('Checkout'){
            steps{
                git: 'https://github.com/criszzamora/dev-ops_personal.git'
            }
        }
        stage('Build Docker image'){
            steps{
                script{
                    sh 'docker build -t criszzamora/dev-ops_personal .'
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarQube Scanner'
                    withSonarQubeEnv('SonarQube Server') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        stage('Push Docker image to Hub'){  
            steps{
                script{
                   withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'pwd-dockerhub', usernameVariable: 'user-dockerhub')]) {
                   sh 'docker login -u ${user-dockerhub} -p ${pwd-dockerhub}'
                   sh 'docker push criszzamora/dev-ops_personal'
                }
            }
        }
        stage('Deploy Docker to Kubernetes'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deployment.yaml',kubeconfigId: 'k8sconfig')
                }
            }
        }
    }
}