pipeline {
    agent any
    environment {
        GITHUB_PAT = 'POC_token'
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

stage('Upload Results to Github') {
    environment {
        CODEQL_PATH = "$WORKSPACE"
        GH_USERNAME = "KaranChadha10"
        REPO_OWNER = "KaranChadha10"  // Replace with the actual repository owner's name
        REPO_NAME = "codeql-analysis"
        BRANCH_TO_SCAN = "master"
    }

    steps {
        script {
            echo "CODEQL_PATH: ${env.CODEQL_PATH}" 
            def sarifFile = "${CODEQL_PATH}/codeql-results.sarif"
            
            if (fileExists(sarifFile)) {
                echo "SARIF file found."
                
                def command = "${CODEQL_PATH}/codeql github upload-results " +
                              "--repository=${REPO_OWNER}/${REPO_NAME} " +
                              "--ref=refs/heads/${BRANCH_TO_SCAN} " +
                              "--commit=${GIT_COMMIT} " +
                              "--sarif=${sarifFile}"
                
                withCredentials([
                    usernamePassword(credentialsId: 'POC_token', 
                                    usernameVariable: 'GH_USERNAME', 
                                    passwordVariable: 'GH_TOKEN')
                ]) {
                    echo "Using username: ${GH_USERNAME}"
                    
                    withEnv(["GITHUB_USERNAME=${GH_USERNAME}", 
                             "GITHUB_TOKEN=${GH_TOKEN}"]) {
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