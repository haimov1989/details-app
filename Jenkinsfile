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
        stage('Spellcheck and Codespell') {
            steps {
                sh '''
                export PATH="${POETRY_HOME}/bin:${PATH}"
                poetry run codespell $(find . -path ./.poetry -prune -o -name "*.py" -o -name "*.txt" -o -name "*.md" -print)
                '''
            }
        }
        stage('Shellcheck') {
            steps {
                sh '''
                export PATH="${POETRY_HOME}/bin/${PATH}"
                poetry run shellcheck $(find . -path ./.poetry -prune -o -name "*.sh" -print)
                '''
            }
        }
        stage('Tests') {
            steps {
                sh '''
                export PATH="${POETRY_HOME}/bin/${PATH}"
                poetry run pytest
                '''
            }
        }
        stage('Build') {
            steps {
                sh '''
                export PATH="${POETRY_HOME}/bin/${PATH}"
                docker build -t your_image:latest .
                docker push your_image:latest
                '''
            }
        }
    }
}
