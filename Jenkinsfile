pipeline {
    agent any
    tools {
        maven "maven-3.6"
    }
    stages {
        stage('increment version'){
            steps{
                script{
                    echo 'incrementing app version...'
                    sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} versions:commit'
                    def marcher=readFile('pom.xml')=~ '<version>(.+)</version>'
                    def version=matcher[0][1]
                    env.IMAGE_NAME= "$version-$BUILD_NUMBER"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    echo "Testing the application.."
                    echo "Executing pipeline for branch $BRANCH_NAME"
                }
            }
        }
        stage('Build app') {
            steps {
                 script {
                    echo "Build the application.."
                    sh 'mvn clean package'
                }
            }
        }
        stage('Buid image') {
            steps {
                 script {
                    echo 'Building the docker image'
                    withCredentials([usernamePassword(credentialsId:'docker-hub-repo', passwordVariable: 'PASS',usernameVariable:'USER' )]){
                        sh "docker build -t alexpatroi/my-jenkins:${IMAGE_NAME} ."
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh "docker push alexpatroi/my-jenkins:${IMAGE_NAME}"
                    }
                }
            }
        }
    }
}