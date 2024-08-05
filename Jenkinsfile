pipeline {
    agent any
    environment {
        POETRY_HOME = "${WORKSPACE}/.poetry"
        PATH = "${POETRY_HOME}/bin:${PATH}"
    }
    stages {
        stage('Setup Poetry') {
            steps {
                sh '''
                curl -sSL https://install.python-poetry.org | python3 -
                export PATH="${POETRY_HOME}/bin:${PATH}"
                poetry lock --no-update
                poetry install
                '''
            }
        }
        stage('Spellcheck') {
            steps {
                sh '''
                export PATH="${POETRY_HOME}/bin:${PATH}"
                poetry run python -m spellchecker -f $(find . -name "*.py" -o -name "*.txt" -o -name "*.md")
                '''
            }
        }
        stage('Codespell and Shellcheck') {
            steps {
                sh '''
                export PATH="${POETRY_HOME}/bin:${PATH}"
                poetry run codespell $(find . -name "*.py")
                poetry run shellcheck $(find . -name "*.sh")
                '''
            }
        }
        stage('Tests') {
            steps {
                sh '''
                export PATH="${POETRY_HOME}/bin:${PATH}"
                poetry run pytest
                '''
            }
        }
        stage('Build') {
            steps {
                sh '''
                docker build -t your_image:latest .
                docker push your_image:latest
                '''
            }
        }
    }
}
