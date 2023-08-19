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

        stage('Upload Results to Github') {
        environment {
            CODEQL_PATH = "$WORKSPACE/build/codeql"
        }

        steps {
            dir("build") {
                bat """
                    for %%result_file in (*.sarif) do (
                        %CODEQL_PATH%/codeql github upload-results ^
                            --repository=KaranChadha10/codeql-analysis ^
                            --ref="refs/heads/master" ^
                            --commit=%GIT_COMMIT% ^
                            --sarif=%%result_file
                    )
                """
            }
        }
    }


     
    }
}