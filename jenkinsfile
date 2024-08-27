pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    def commitSha = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
                    def repoName = sh(returnStdout: true, script: 'git config --get remote.origin.url').trim().split('/').last().replace('.git', '')

                    withCredentials([string(credentialsId: 'AzIsra', variable: 'AzIsra')]) {
                        sh """
                        curl -s -X POST -H "Authorization: token ${AzIsra}" \
                        -d '{"state": "pending", "target_url": "${env.BUILD_URL}", "description": "Build is in progress", "context": "Build"}' \
                        https://api.github.com/repos/AzIsra/JenkinsTest/statuses/${commitSha}
                        """
                    }

                    // Example build command
                    sh 'make build'
                }
            }

            post {
                success {
                    script {
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
