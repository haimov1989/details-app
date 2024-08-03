pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub') // Ensure Docker Hub credentials are set in Jenkins
    }
    stages {
        stage('Spell Check') {
            steps {
                script {
                    // Simulate spell check
                    writeFile file: 'spellcheck.sh', text: '''
                    #!/bin/bash
                    echo "Running spell check..."
                    # Add actual spellcheck commands here
                    '''
                    sh 'chmod +x spellcheck.sh'
                    sh './spellcheck.sh'
                }
            }
        }
        stage('Codespell') {
            steps {
                script {
                    // Simulate codespell
                    writeFile file: 'codespell.sh', text: '''
                    #!/bin/bash
                    echo "Running codespell..."
                    # Add actual codespell commands here
                    '''
                    sh 'chmod +x codespell.sh'
                    sh './codespell.sh'
                }
            }
        }
        stage('Shellcheck') {
            steps {
                script {
                    // Simulate shellcheck
                    writeFile file: 'shellcheck.sh', text: '''
                    #!/bin/bash
                    echo "Running shellcheck..."
                    # Add actual shellcheck commands here
                    '''
                    sh 'chmod +x shellcheck.sh'
                    sh './shellcheck.sh'
                }
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    // Simulate tests
                    writeFile file: 'tests/test_sample.py', text: '''
                    def test_sample():
                        assert 1 + 1 == 2
                    '''
                    writeFile file: 'requirements.txt', text: '''
                    pytest
                    '''
                    sh 'pip install -r requirements.txt'
                    sh 'pytest tests/'
                }
            }
        }
        stage('Build Container') {
            steps {
                script {
                    // Create Dockerfile
                    writeFile file: 'Dockerfile', text: '''
                    FROM python:3.8-slim
                    COPY . /app
                    WORKDIR /app
                    RUN pip install -r requirements.txt
                    CMD ["python", "app.py"]
                    '''
                    // Simulate building the container
                    sh 'docker build -t my-app .'
                }
            }
        }
        stage('Save to Docker Hub') {
            steps {
                script {
                    // Log in to Docker Hub and push the image
                    sh 'echo $DOCKER_HUB_CREDENTIALS_PSW | docker login -u $DOCKER_HUB_CREDENTIALS_USR --password-stdin'
                    sh 'docker tag my-app:latest my-app:latest'
                    sh 'docker push my-app:latest'
                }
            }
        }
    }
    post {
        always {
            script {
                // Clean up
                sh 'docker rmi my-app:latest'
            }
        }
    }
}
