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
            when {
                expression{
                    BRANCH_NAME== 'main'
                }
            }
            steps {
                 script {
                    echo "Testing the application.."
                }
            }
        }
        stage('Deploy') {
            when {
                expression{
                    BRANCH_NAME== 'main'
                }
            }
            steps {
                 script {
                    echo "Testing the application.."
                }
            }
        }
    }
}