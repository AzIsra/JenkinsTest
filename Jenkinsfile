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
                    githubNotify context: 'Test', status: 'SUCCESS', description: 'All tests passed', targetUrl: "https://github.com/AzIsra/JenkinsTest.git"
                }

                failure {
                    githubNotify context: 'Test', status: 'FAILURE', description: 'Some tests failed', targetUrl: "https://github.com/AzIsra/JenkinsTest.git"
                }
        }
    }
}
