pipeline {
    agent any

    environment {
        PATH = "$HOME/.local/bin:$PATH"
        PYTHONPATH = "${WORKSPACE}/src"
    }

    stages {
        stage('Pre-Build') {
            steps {
                echo 'Checking pre-requisites'
                sleep 2
                sh '''
                    sudo apt-get update
                    sudo apt-get install -y wget curl python3 python3-pip python3-venv python3-flask pipenv pylint pipx
                    pipx install pyinstaller
                    curl -sSL https://install.python-poetry.org | python3 -
                '''
            }
        }
        stage('Setup Environment') {
            steps {
                sh '''
                    export PATH="$HOME/.local/bin:$PATH"
                    poetry install
                '''
            }
        }
        stage('Lint Code') {
            steps {
                sh '''
                    export PATH="$HOME/.local/bin:$PATH"
                    export PYTHONPATH="${WORKSPACE}/src"
                    poetry run pylint --disable=missing-docstring,invalid-name src/details/app.py
                '''
            }
        }
        // Add other stages as needed
    }
}
