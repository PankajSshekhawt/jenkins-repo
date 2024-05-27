pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'g++ -o my_program *.cpp'
                    } else {
                        bat 'g++ -o my_program *.cpp'
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    if (isUnix()) {
                        sh './my_program'
                    } else {
                        bat 'my_program.exe'
                    }
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'my_program', allowEmptyArchive: true
            junit 'test-results/*.xml' // If you have test results to publish
        }
    }
}
