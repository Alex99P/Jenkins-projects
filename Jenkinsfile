pipeline {
    agent any
    tools {
        maven "maven-3.6"
    }
    stages {
         stage('increment version') {
            steps {
                script {
                    echo 'incrementing app version...'
                    sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER"
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
         stage('commit version update ') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'githunconn', variable: 'TOKEN')]) {
                        // git config here for the first time run
                        sh 'git config --global user.email "jenkins@example.com"'
                        sh 'git config --global user.name "jenkins"'

                        sh "git remote set-url origin https://$TOKEN@github.com/Alex99P/Jenkins-projects.git"
                        sh 'git add .'
                        sh 'git commit -m "ci: version bump"'
                        sh 'git push origin HEAD:versioning'
                    }
                }
            }
        

    }
}
}