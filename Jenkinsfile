pipeline {
    agent any

    stages {
        stage('Install Poetry') {
            steps {
                sh '''
                curl -sSL https://install.python-poetry.org | python3 -
                export PATH="$HOME/.local/bin:$PATH"
                '''
            }
        }
        stage('Install Dependencies') {
            steps {
                sh '''
                export PATH="$HOME/.local/bin:$PATH"
                poetry install
                '''
            }
        }
        stage('Run Spellcheck') {
            steps {
                sh '''
                export PATH="$HOME/.local/bin:$PATH"
                poetry run codespell .
                '''
            }
        }
    }
}
