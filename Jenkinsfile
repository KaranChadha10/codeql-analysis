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
            def apiUrl = 'https://api.github.com/user'
            
                            withCredentials([(
                    usernamePassword(credentialsId: env.GITHUB_PAT, 
                                    usernameVariable: 'KaranChadha10', 
                                    passwordVariable: 'GH_TOKEN')
                )]) {
                //  withCredentials([
                //     usernamePassword(credentialsId: 'Pat_12', 
                //                     usernameVariable: 'GH_USERNAME', 
                //                     passwordVariable: 'GH_TOKEN')
                // ]) {
                def response = httpRequest(
                url: apiUrl,
                httpMode: 'GET',
                authenticationType: 'Bearer',
                customHeaders: [[name: 'Authorization', value: "Bearer ${GH_TOKEN}"]]
            )
                echo response
                echo "Response Content: ${response.content}"
                    if (response.status == 200) {
                    def responseBody = response.content
                    echo "Response Content: ${responseBody}"
                } else {
                    error "API call failed with status ${response.status}"
                }
            }
        }
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
        // stage('Publish CodeQL Results') {
        //     steps {
        //         // Publish CodeQL analysis results using the CodeQL plugin
        //         codeql(codeqlConfig: 'MyCodeQLDatabase', sarifPattern: 'codeql-results.sarif')
        //     }
        // }
        // stage('Publish CodeQL Results') {
        //     steps {
        //         // Publish CodeQL analysis results using the CodeQL plugin
        //         recordIssues(
        //             tools: [
        //                 codeQL(
        //                     pattern: 'codeql-results.sarif', 
        //                     reportEncoding: 'UTF-8'
        //                 )
        //             ]
        //         )
        //     }
        // } 
        // stage('Upload Results to Github') {
        //     environment {
        //         CODEQL_PATH = "$WORKSPACE/build/codeql"
        //     }
        //     steps {
        //         dir("build") {
        //             sh """
        //                 for result_file in *.sarif
        //                 do
        //                     $CODEQL_PATH/codeql github upload-results \\
        //                         --repository=KaranChadha10/codeql-analysis \\
        //                         --ref="refs/heads/master" \\
        //                         --commit=${GIT_COMMIT} \\
        //                         --sarif=\${result_file}
        //                 done
        //             """
        //         }
        //     }
        // }
//         stage('Upload Results to Github') {
//     environment {
//         CODEQL_PATH = "$WORKSPACE/build/codeql"
//     }

//     steps {
//         dir("build") {
//             withCredentials([usernamePassword(credentialsId: env.JCI_GHEMU_CRED,
//                                               usernameVariable: 'GHUSER',
//                                               passwordVariable: 'GHTOKEN')]) {
//                 bat """
//                     for %%result_file in (*.sarif) do (
//                         echo %GHTOKEN% | ^
//                           %CODEQL_PATH%/codeql github upload-results ^
//                             --repository=KaranChadha10/codeql-analysis ^
//                             --ref="refs/heads/master" ^
//                             --commit=%GIT_COMMIT% ^
//                             --sarif=%%result_file
//                     )
//                 """
//             }
//         }
//     }
// }

// stage('Upload Results to Github') {
//     environment {
//         CODEQL_PATH = "$WORKSPACE/build/codeql"
//     }

//     steps {
//         dir("build") {
//             bat """
//                 for %%result_file in (*.sarif) do (
//                     %CODEQL_PATH%/codeql github upload-results ^
//                         --repository=KaranChadha10/codeql-analysis ^
//                         --ref="refs/heads/master" ^
//                         --commit=%GIT_COMMIT% ^
//                         --sarif=%%result_file
//                 )
//             """
//         }
//     }
// }

    //     stage('Upload Results to Github') {
    //     environment {
    //         CODEQL_PATH = "$WORKSPACE/build/codeql"
    //     }

    //     steps {
    //         dir("build") {
    //             bat """
    //                 for %%result_file in (*.sarif) do (
    //                     %CODEQL_PATH%/codeql github upload-results ^
    //                         --repository=KaranChadha10/codeql-analysis ^
    //                         --ref="refs/heads/master" ^
    //                         --commit=%GIT_COMMIT% ^
    //                         --sarif=%%result_file
    //                 )
    //             """
    //         }
    //     }
    // }

        //     stage('Upload Results to Github') {
        //     environment {
        //         CODEQL_PATH = "$WORKSPACE/build/codeql"
        //     }

        //     steps {
        //         dir("build") {
        //             script {
        //                 def resultFiles = findFiles(glob: '*.sarif')

        //                 for (resultFile in resultFiles) {
        //                     def command = "${CODEQL_PATH}/codeql github upload-results " +
        //                                 "--repository=KaranChadha10/codeql-analysis " +
        //                                 "--ref=refs/heads/master " +
        //                                 "--commit=${GIT_COMMIT} " +
        //                                 "--sarif=${resultFile.getName()}"
                            
        //                     bat(command)
        //                 }
        //             }
        //         }
        //     }
        // }
    //         stage('Upload Results to Github') {
    //     environment {
    //         CODEQL_PATH = "$WORKSPACE/build/codeql"
    //     }

    //     steps {
    //         dir("build") {
    //             script {
    //                 def resultFiles = bat(script: 'dir /B *.sarif', returnStatus: true).trim()
    //                 def filesList = resultFiles.split('\r\n')

    //                 for (resultFile in filesList) {
    //                     def command = "${CODEQL_PATH}/codeql github upload-results " +
    //                                 "--repository=KaranChadha10/codeql-analysis " +
    //                                 "--ref=refs/heads/master " +
    //                                 "--commit=${GIT_COMMIT} " +
    //                                 "--sarif=${resultFile}"
                        
    //                     bat(command)
    //                 }
    //             }
    //         }
    //     }
    // }
        //     stage('Upload Results to Github') {
        //     environment {
        //         CODEQL_PATH = "$WORKSPACE/build/codeql"
        //     }

        //     steps {
        //         script {
        //             def resultFiles = findFiles(glob: '**/*.sarif')
                    
        //             for (resultFile in resultFiles) {
        //                 def command = "${CODEQL_PATH}/codeql github upload-results " +
        //                             "--repository=KaranChadha10/codeql-analysis " +
        //                             "--ref=refs/heads/master " +
        //                             "--commit=${GIT_COMMIT} " +
        //                             "--sarif=${resultFile}"
                        
        //                 bat(command)
        //             }
        //         }
        //     }
        // }

//         stage('Upload Results to Github') {
//     environment {
//         CODEQL_PATH = "$WORKSPACE/build/codeql"
//     }

//     steps {
//         script {
//             def resultFiles = findFiles(glob: '**/*.sarif')
            
//             if (!resultFiles.empty) {
//                 resultFiles.each { resultFile ->
//                     def command = "${CODEQL_PATH}/codeql github upload-results " +
//                                   "--repository=KaranChadha10/codeql-analysis " +
//                                   "--ref=refs/heads/master " +
//                                   "--commit=${GIT_COMMIT} " +
//                                   "--sarif=${resultFile}"

//                     bat(command)
//                 }
//             } else {
//                 echo "No SARIF files found."
//             }
//         }
//     }
// }
        

// stage('Upload Results to Github') {
//     environment {
//         CODEQL_PATH = "$WORKSPACE"
//         GH_TOKEN = "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"
//     }

//     steps {
//         script {
//             echo "CODEQL_PATH: ${env.CODEQL_PATH}" 
//             def sarifFile = "${CODEQL_PATH}/codeql-results.sarif"
            
//             if (fileExists(sarifFile)) {
//                 def patCheckCommand = "curl -s -w %{http_code} -H \"Authorization: token ${GH_TOKEN}\" https://api.github.com/user"
//                 echo "patCheckCommand: ${patCheckCommand}" // Add this line
//                 def patCheckResult = bat(script: "cmd /c \"${patCheckCommand}\"", returnStatus: true).trim()
//                 echo "patCheckResult: ${patCheckResult}" // Add this line

//                 if (patCheckResult == '200') { // Compare as a string, not integer
//                     def command = "codeql github upload-results " +
//                                   "--repository=KaranChadha10/codeql-analysis " +
//                                   "--ref=refs/heads/main " +
//                                   "--commit=${GIT_COMMIT} " +
//                                   "--sarif=${sarifFile}"

//                     bat(script: command)
//                 } else {
//                     error "Invalid PAT: Received ${patCheckResult} Unauthorized"
//                 }
//             } else {
//                 echo "SARIF file not found."
//             }
//         }
//     }
// }






    }
}