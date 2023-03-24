# Dynamically Increment Application version in Jenkins Pipeline

## Technologies used:
**Jenkins, Docker, GitHub, Git, Java, Maven**

## Project Description:

* Configure CI step: 
    - Increment patch version
    - Build Java application and clean old artifacts
    - Build Image with dynamic Docker Image Tag
    - Push Image to private DockerHub repository
    - Commit version update of Jenkins back to Git repository

* Configure Jenkins pipeline to not trigger automatically on CI build commit to avoid commit loop