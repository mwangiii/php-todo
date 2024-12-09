pipeline {
    agent any
    tools {
        // Use the correct JDK tool name (if it's configured globally)
        jdk 'java 11'  // Ensure this is the correct tool name from Global Tool Configuration
    }
    environment {
        // You can explicitly set JAVA_HOME if required
        JAVA_HOME = '/usr/lib/jvm/java-11-openjdk-amd64'
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
    }
    
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
                scannerHome = tool 'SonarQubeScanner' // Make sure this is correctly configured in Jenkins
            }
            steps {
                withSonarQubeEnv('sonarQube') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
    }
}
