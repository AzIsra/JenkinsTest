pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building..3'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
            post {
                success {
                    githubNotify context: 'Test', status: 'SUCCESS', description: 'All tests passed', targetUrl: "${env.BUILD_URL}"
                }

                failure {
                    githubNotify context: 'Test', status: 'FAILURE', description: 'Some tests failed', targetUrl: "${env.BUILD_URL}"
                }
        }
    }
}
