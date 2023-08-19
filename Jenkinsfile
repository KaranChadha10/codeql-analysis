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
        stage('Download CodeQL CLI') {
            steps {
                sh '''
                    cd build
                    curl -LO https://github.com/github/codeql-action/releases/latest/download/codeql-bundle-linux64.tar.gz
                    tar xvf codeql-bundle-linux64.tar.gz
                '''
            }
        }
    }
}