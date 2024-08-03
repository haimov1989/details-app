pipeline {
    agent any
    stages {
        stage('Spell Check') {
            steps {
                script {
                    def response = sh(script: "curl -X POST http://localhost:8080/job/spellcheck/build --user <username>:<api-token>", returnStdout: true)
                    echo "Response: ${response}"
                }
            }
        }
        stage('Codespell and Shellcheck') {
            steps {
                script {
                    def response = sh(script: "curl -X POST http://localhost:8080/job/codespell-shellcheck/build --user <username>:<api-token>", returnStdout: true)
                    echo "Response: ${response}"
                }
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    def response = sh(script: "curl -X POST http://localhost:8080/job/tests/build --user <username>:<api-token>", returnStdout: true)
                    echo "Response: ${response}"
                }
            }
        }
        stage('Build Container') {
            steps {
                script {
                    def response = sh(script: "curl -X POST http://localhost:8080/job/build-container/build --user <username>:<api-token>", returnStdout: true)
                    echo "Response: ${response}"
                }
            }
        }
    }
    post {
        always {
            script {
                def response = sh(script: "curl -X POST http://localhost:8080/job/save-to-dockerhub/build --user <username>:<api-token>", returnStdout: true)
                echo "Response: ${response}"
            }
        }
    }
}
