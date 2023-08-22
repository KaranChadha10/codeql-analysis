pipeline {
    agent any
    environment {
        GITHUB_PAT = 'Pat_11'
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
        stage('GitHub API Call') {
            environment {
            // GH_TOKEN = "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"
            GITHUB_PAT = 'PAT_11' // Use the correct credentials ID here

        }
    steps {
        script {
            def apiUrl = 'https://jsonplaceholder.typicode.com/posts'
            def PAT = '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'

            def response = httpRequest(
                        url: apiUrl,
                        httpMode: 'GET',
                        authentication: patToken,
                        customHeaders: [[name: 'Authorization', value: "${PAT}"]]
                    )
            echo response
                echo "Response Code: ${response.status}"
                echo "Response Content: ${response.content}"
        }
    }
    }
//         stage('GitHub API Call') {
//             environment {
//             // GH_TOKEN = "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"
//             GITHUB_PAT = 'PAT_11' // Use the correct credentials ID here

//         }
//     steps {
        
//         script {
//             def apiUrl = 'https://api.github.com/user'
            
//                             withCredentials([(
//                     usernamePassword(credentialsId: env.GITHUB_PAT, 
//                                     usernameVariable: 'KaranChadha10', 
//                                     passwordVariable: 'GH_TOKEN')
//                 )]) {
//                 //  withCredentials([
//                 //     usernamePassword(credentialsId: 'Pat_12', 
//                 //                     usernameVariable: 'GH_USERNAME', 
//                 //                     passwordVariable: 'GH_TOKEN')
//                 // ]) {
//                 def response = httpRequest(
//                 url: apiUrl,
//                 httpMode: 'GET',
//                 authenticationType: 'Bearer',
//                 customHeaders: [[name: 'Authorization', value: "Bearer ${GH_TOKEN}"]]
//             )
//                 echo response
//                 echo "Response Content: ${response.content}"
//                     if (response.status == 200) {
//                     def responseBody = response.content
//                     echo "Response Content: ${responseBody}"
//                 } else {
//                     error "API call failed with status ${response.status}"
//                 }
//             }
//         }
//     }
// }
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
        GH_USERNAME = 'KaranChadha10'
        GH_TOKEN = "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"
    }

    steps {
        script {
            echo "CODEQL_PATH: ${env.CODEQL_PATH}" 
            echo GHUER
            def sarifFile = "${CODEQL_PATH}/codeql-results.sarif"
            
            if (fileExists(sarifFile)) {
                    def command = "codeql github upload-results " +
                                  "--repository=KaranChadha10/codeql-analysis " +
                                  "--ref=refs/heads/main " +
                                  "--commit=${GIT_COMMIT} " +
                                  "--sarif=${sarifFile}"
                    withCredentials([
                    usernamePassword(credentialsId: 'Pat_12', 
                                    usernameVariable: 'GH_USERNAME', 
                                    passwordVariable: 'GH_TOKEN')
                ]) {
                    bat(script: command)
                }
            } else {
                echo "SARIF file not found."
            }
        }
    }
}






    }
}