pipeline {
    agent any
    environment {
        GITHUB_PAT = 'pat_12'
    }

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
        stage('Checkout') {
            steps {
                // Checkout code from Git
                checkout([$class: 'GitSCM',
                        branches: [[name: '*/master']], // Use 'master' for the main branch
                        userRemoteConfigs: [[credentialsId: env.GITHUB_PAT,
                                            url: 'https://github.com/KaranChadha10/codeql-analysis.git']]])
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
                }
            }
        }
        

stage('Upload Results to Github') {
    environment {
        CODEQL_PATH = "$WORKSPACE"
        GH_USERNAME = "KaranChadha10"
        GH_TOKEN = "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"
    }

    steps {
        script {
            echo "CODEQL_PATH: ${env.CODEQL_PATH}" 
            def sarifFile = "${CODEQL_PATH}/codeql-results.sarif"
            
            if (fileExists(sarifFile)) {
                def command = "codeql github upload-results " +
                              "--repository=KaranChadha10/codeql-analysis " +
                              "--ref=refs/heads/main " +
                              "--commit=${GIT_COMMIT} " +
                              "--sarif=${sarifFile}"

                withCredentials([
                    usernamePassword(credentialsId: "pat_12", 
                                    usernameVariable: "GH_USERNAME", 
                                    passwordVariable: "GH_TOKEN")
                ]) {

                    // Pass environment variables to the withEnv block
                    withEnv(["GITHUB_USERNAME=${env.GH_USERNAME}", 
                             "GITHUB_TOKEN=${env.GH_TOKEN}"]) {
                        bat(script: command)
                    }
                }
            } else {
                echo "SARIF file not found."
            }
        }
    }
}






    }
}