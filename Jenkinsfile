pipeline {
    agent any
    environment {
        NEW_VERSION='1.3.0'
        // SERVER_CREDENTIALS = credentials('server-credentials')
    }

    stages {
        stage('Build') {
             when {
                expression {
                    BRANCH_NAME == 'dev' && CODE_CHANGES == true
                }
            }
            steps {
                echo 'Building..'
                echo "version ${NEW_VERSION}"
            }
        }
        stage('Test') {
            when {
                expression {
                    BRANCH_NAME == 'dev' || BRANCH_NAME == 'master'
                }
            }
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                withCredentials([
                    usernamePassword(credentials: 'server-credentials', usernameVariable: USER, passwordVariable: PWD)
                ]) {
                 sh "some script ${USER} ${PWD}"
                }
            }
        }
    }
}