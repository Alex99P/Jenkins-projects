# Jenkins project
## Technologies used:
**Jenkins, Docker, GitHub, Git, Java, Maven**

## Project Description:

### Create a CI Pipeline with Jenkinsfile (Freestyle, Pipeline, Multibranch Pipeline)

* CI Pipeline for a Java Maven application to build and push to the repository
* Install Build Tools (Maven, Node) in Jenkins
* Make Docker available on Jenkins server
* Create Jenkins credentials for a git repository
* Create different Jenkins job types (Freestyle, Pipeline, Multibranchpipeline) for the Java Maven project with Jenkinsfile to:
    - Connect to the application’s git repository
    - Build Jar
    - Build Docker Image
    - Push to private DockerHub repository

### Configure Webhook to trigger CI Pipeline automatically on every change 
* Install GitHub Plugin in Jenkins
* Configure GitHub access token and connection to
* Jenkins in GitHub project settings
* Configure Jenkins to trigger the CI pipeline, whenever a
* change is pushed to GitHub

### Dynamically Increment Application version in Jenkins Pipeline
* Configure CI step: 
    - Increment patch version
    - Build Java application and clean old artifacts
    - Build Image with dynamic Docker Image Tag
    - Push Image to private DockerHub repository
    - Commit version update of Jenkins back to Git repository
* Configure Jenkins pipeline to not trigger automatically on CI build commit to avoid commit loop