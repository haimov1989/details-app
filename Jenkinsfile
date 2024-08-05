pipeline {
    agent any
    stages {
        stage('Pre-Build Setup') {
            steps {
                sh '''
                    echo "Installing necessary tools..."
                    sudo apt-get update
                    sudo apt-get install -y aspell
                    sudo apt-get install -y shellcheck
                    pip install poetry codespell pytest
                '''
            }
        }
        stage('Spellcheck') {
            steps {
                sh '''
                    echo "Running aspell spellcheck..."
                    # Run aspell on all .txt files in the repository
                    for file in $(find . -name "README.md"); do
                        aspell check "$file"
                    done
                '''
            }
        }
        stage('Codespell') {
            steps {
                sh '''
                    echo "Running codespell..."
                    # Run codespell
                    codespell .
                '''
            }
        }
        stage('Shellcheck') {
            steps {
                sh '''
                    echo "Running shellcheck..."
                    # Run shellcheck on all shell scripts
                    find . -name "*.sh" -exec shellcheck {} \;
                '''
            }
        }
        stage('Tests') {
            steps {
                sh '''
                    echo "Running tests..."
                    # Install dependencies
                    poetry install
                    # Run pytest
                    poetry run pytest
                '''
            }
        }
        stage('Build') {
            steps {
                sh '''
                    echo "Building container..."
                    # Build Docker image
                    docker build -t your-dockerhub-username/your-image-name:tag .
                '''
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh '''
                        echo "Pushing image to Docker Hub..."
                        docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
                        docker push your-dockerhub-username/your-image-name:tag
                    '''
                }
            }
        }
    }
}
