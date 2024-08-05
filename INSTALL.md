# Installation Instructions

## Prerequisites
- Jenkins server with necessary plugins installed.
- Docker installed on the Jenkins worker.

## Setup
1. Clone the repository.
2. Configure Jenkins to use the provided `Jenkinsfile`.
3. Ensure all dependencies are installed.

## Running the Project
- Trigger the Jenkins pipeline from the Jenkins UI or using the `curl` command:
```sh
curl -X POST http://localhost:8080/job/details_app_jenkins/build --user 'admin:API_TOKEN'
