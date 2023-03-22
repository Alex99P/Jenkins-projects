def gv
pipeline {
    agent any
    parameters {
        // string(name: 'VERSION', defaultValue:'' , description: '' )
        choice(name: 'VERSION', choices: ['1.1.0','1.2.0','1.3.0'], description:"")
        booleanParam(name:'executeTests', defaultValue: true, description: '')

    }

    stages {
        stage('init') {
            steps {
                script {
                   gv= load "script.groovy" 
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    gv.buildApp()
                }
            }
        }
        stage('Test') {
            when {
                expression {
                   params.executeTests
                }
            }
            steps {
                script {
                    gv.buildApp()
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // env.ENV = input message: "Select the env to deploy to", ok:"Done", parameters: choice(name: 'ENV', choices: ['dev','staging','prod'], description:"")
                    gv.deployApp()
                    // echo "Deploying to ${ENV}"
                }
            }
        }
    }
}