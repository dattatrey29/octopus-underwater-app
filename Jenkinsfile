pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
         stage('Clone repository') { 
            steps { 
                script{
                checkout scm
                }
            }
        }
        
        stage('Build') { 
            steps { 
                script{
                 app = docker.build("underwater")
                }
            }
        }
        stage('Test'){
            steps {
                 echo 'Empty'
            }
        }
        stage('Push') {
            steps {
                script{
                        docker.withRegistry('https://449025498404.dkr.ecr.ap-south-1.amazonaws.com', 'ecr:ap-south-1:aws-kainskep') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                    }
                }
            }
        }
        stage('Deploy'){
            steps {
                 bat 'kubectl apply -f deployment.yml'
                 bat 'kubectl rollout restart deployment ecr-app-underwater'
            }
        }
        
    }
}
