pipeline {
    agent any
    stages {
        stage("Initial cleanup") {
            steps {
                dir("${WORKSPACE}") {
                    deleteDir()
                }
            }
        }

        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/mwangiii/php-todo.git'
            }
        }


        stage('SonarQube Quality Gate') {
            environment {
                scannerHome = tool 'SonarQubeScanner'
            }
            steps {
                withSonarQubeEnv('sonarQube') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
    }
}
