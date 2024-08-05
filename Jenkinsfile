pipeline {
    agent any

    environment {
        PATH = "$HOME/.local/bin:$PATH"
    }

    stages {
        stage('Install Poetry') {
            steps {
                sh '''
                curl -sSL https://install.python-poetry.org | python3 -
                '''
            }
        }
        stage('Update Poetry Lock File') {
            steps {
                sh '''
                poetry lock --no-update
                '''
            }
        }
        stage('Install Dependencies') {
            steps {
                sh '''
                poetry install
                '''
            }
        }
        stage('Run Spellcheck') {
            steps {
                sh '''
                poetry run codespell .
                '''
            }
        }
        stage('Run Shellcheck') {
            steps {
                sh '''
                echo "Running shellcheck..."
                find . -name "*.sh" -exec shellcheck {} \;
                '''
            }
        }
        stage('Run Tests') {
            steps {
                sh '''
                poetry run pytest .
                '''
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t yourdockerhubusername/yourimage:latest .'
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                    sh 'echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USERNAME --password-stdin'
                    sh 'docker push yourdockerhubusername/yourimage:latest'
                }
            }
        }
    }
}
