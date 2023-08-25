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
                        // def command = "codeql github upload-results " +
                        //             "--repository=KaranChadha10/codeql-analysis " +
                        //             "--ref=refs/heads/master " +
                        //             "--commit=${GIT_COMMIT} " +
                        //             "--sarif=${sarifFile}"

                        def command = "codeql github upload-results " +
                                    "--github-url=https://api.github.com " +
                                    "--repository=KaranChadha10/codeql-analysis " +
                                    "--ref=refs/heads/master " +
                                    "--commit=${GIT_COMMIT} " +
                                    "--sarif=${sarifFile}"

                        withCredentials([
                            usernamePassword(credentialsId: "pat_12", 
                                            usernameVariable: 'GH_USERNAME', 
                                            passwordVariable: 'GH_TOKEN')
                        ]) {
                            echo "Username ${env.GH_USERNAME}"
                            echo "Password: ${env.GH_TOKEN}" 
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

        // stage('Upload Results to Github') {
        //             environment {
        //                 CODEQL_PATH = "${WORKSPACE}\\codeql"
        //                 GH_USERNAME = "jtripade_jcplc"  // Your GitHub username
        //                 GH_TOKEN = "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"  // Your GitHub personal access token

        //             }
        //             steps {
        //                 script {
        //                     def sarifFile = "${CODEQL_PATH}\\codeql-db\\results-js.sarif"
        //                     if (fileExists(sarifFile)) {
        //                         def command = "${CODEQL_PATH}\\codeql github upload-results " +
        //                             "--github-url=https://api.github.com " +
        //                             "--repository=${OWNER_REPO_NAME} " +
        //                             "--ref=refs/heads/${BRANCH_TO_SCAN} " +
        //                             "--commit=${GIT_COMMIT} " +
        //                             "--sarif=${sarifFile}"

        //                         withCredentials([usernamePassword(credentialsId: env.JCI_GHEMU_CRED,
        //                                     usernameVariable: 'GHUSER',
        //                                     passwordVariable: 'GHTOKEN')]) {
        //                             def authHeaderValue = "Bearer ${env.GH_TOKEN}"  // Use GH_TOKEN here
        //                             withEnv(["GITHUB_USERNAME=${env.GH_USERNAME}", 
        //                                     "GITHUB_TOKEN=${env.GH_TOKEN}"]) {
        //                                 bat(script: command, returnStatus: true, env: [GITHUB_AUTH: authHeaderValue])
        //                             }
        //                         }
        //                     } else {
        //                         echo "SARIF file not found."
        //                     }
        //                 }
        //             }
        //         }
    }
}