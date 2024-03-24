pipeline{
    agent any
    stages {
        stage('Git Checkout'){
            steps {
                git branch: 'main', url: 'https://github.com/Niharikaareddi/Letsmeet-DevOps-Project'
            }
        }
        stage('Trivy FileSystem scan') {
            steps {
                sh "trivy fs . > trivyfs.txt"
            }
        }
        stage('Build & Tag Docker Image') {
            steps{
                script{
                    withDockerRegistry(credentialsId: 'docker-cred' , toolName: 'docker') {
                      sh "docker build -t docker.io/niharikareddyy/meetapp:v1 ."
                    }
                }
            }
        }
        stage('Trivy Image Scan') {
            steps {
                sh "trivy image niharikareddyy/meetapp:v1 > trivy.txt"
            }
        }
        stage('push Docker Image'){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'docker-cred' , toolName: 'docker') {
                        sh "docker push docker.io/niharikareddyy/meetapp:v1"
                    }
                }
            }
        }
        stage('Deploy Container') {
            steps {
                sh "docker run -d -p 8081:80 niharikareddyy/meetapp:v1"
            }
        }
    }

}
    
