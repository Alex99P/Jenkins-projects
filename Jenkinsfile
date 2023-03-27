pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                script {
                    echo "Testing the application.."
                    echo "Executing pipeline for branch $BRANCH_NAME"
                }
            }
        }
        stage('Build') {
            steps {
                 script {
                    echo "Testing the application.."
                }
            }
        }
        stage('Deploy') {
            steps {
                 script {
                    def dockerCmd = 'docker run -p3080:3080 -d alexpatroi/reactnodejs:5.1'
                    sshagent(['ec2-sever-key']) {
                     sh "ssh -o StrictHostKeyChecking=no  ec2-user@3.72.106.137 ${dockerCmd}"
                    }
                }
            }
        }
    }
}