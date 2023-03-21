pipeline {
    agent any
    parameters {
        string(name: 'VERSION', defaultValue:'' , description: '' )
        choice(name: 'VERSION', choices: ['1.1.0','1.2.0','1.3.0'], description:"")
        booleanParam(name:'executeTests', defaultValue: true, description: '')

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
            }
        }
        stage('Test') {
            when {
                expression {
                   params.executeTests
                }
            }
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                echo 'deploying version ${params.VERSION} '
            }
        }
    }
}