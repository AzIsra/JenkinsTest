pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    def commitSha = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
                    
                    withCredentials([string(credentialsId: 'AzIsra', variable: 'AzIsra')]) {
                        sh """
                        curl -s -X POST -H "Authorization: token ${AzIsra}" \
                        -d '{"state": "pending", "target_url": "${env.BUILD_URL}", "description": "Build is in progress", "context": "Build"}' \
                        https://api.github.com/repos/AzIsra/JenkinsTest/statuses/${commitSha}
                        """
                    }

                    // Example build command
                    sh 'echo "Hello World"'
                }
            }

            post {
                success {
                    script {
                        def commitSha = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
                        withCredentials([string(credentialsId: 'AzIsra', variable: 'AzIsra')]) {
                            sh """
                            curl -s -X POST -H "Authorization: token ${AzIsra}" \
                            -d '{"state": "success", "target_url": "${env.BUILD_URL}", "description": "Build completed successfully", "context": "Build"}' \
                            https://api.github.com/repos/AzIsra/JenkinsTest/statuses/${commitSha}
                            """
                        }
                    }
                }

                failure {
                    script {
                        def commitSha = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
                        withCredentials([string(credentialsId: 'AzIsra', variable: 'AzIsra')]) {
                            sh """
                            curl -s -X POST -H "Authorization: token ${AzIsra}" \
                            -d '{"state": "failure", "target_url": "${env.BUILD_URL}", "description": "Build failed", "context": "Build"}' \
                            https://api.github.com/repos/AzIsra/JenkinsTest/statuses/${commitSha}
                            """
                        }
                    }
                }
            }
        }
    }
}
