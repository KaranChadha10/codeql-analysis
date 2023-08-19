pipeline {
    agent any

    stages {
        stage('Hello World') {
            steps {
                echo 'Hello world'
            }
        }
        stage('Codeql Analysis') {
            steps {
                echo 'Entering Codeql Analysis'
            }
        }
        // stage('Download CodeQL CLI') {
        //     steps {
        //         sh '''
        //             cd build
        //             curl -LO https://github.com/github/codeql-action/releases/latest/download/codeql-bundle-linux64.tar.gz
        //             tar xvf codeql-bundle-linux64.tar.gz
        //         '''
        //     }
        // }
        stage('Checkout') {
            steps {
                // Checkout code from Git
                checkout([$class: 'GitSCM',
                        branches: [[name: '*/master']], // Use 'master' for the main branch
                        userRemoteConfigs: [[url: 'https://github.com/KaranChadha10/codeql-analysis.git']]])
            }
        }
        stage('CodeQL') {
            steps {
                script {
                    def codeqlExecutable = 'C:\\codeql\\codeql' // Use the correct path to your CodeQL CLI executable
                    def databaseName = 'MyCodeQLDatabase'

                    // Create and analyze CodeQL database
                    bat "${codeqlExecutable} database create --language=javascript --overwrite ${databaseName}" // Add --overwrite flag
                    bat "${codeqlExecutable} database analyze --format=sarif-latest --output=codeql-results.sarif ${databaseName}"

                    // Export results in SARIF format
                    bat "${codeqlExecutable} database export sarif --output=codeql-results.sarif ${databaseName}"
                }
            }
        }   
    }
}