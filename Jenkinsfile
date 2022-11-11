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
                 app = docker.build("octopus-underwater-app")
                }
            }
        }
        stage('Test'){
            steps {
                 echo 'Test '
            }
        }
        stage('Deploy') {
            steps {
                script{
                        docker.withRegistry('https://604790146725.dkr.ecr.sa-east-1.amazonaws.com/octopus-underwater-app', 'ecr:sa-east-1:jenkins-aws') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                    }
                }
            }
        }
    }
    post {
        always {
            slackSend color: "good", message: "Message from Jenkins Pipeline"
        }
    }
}
