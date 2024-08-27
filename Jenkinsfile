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
                setBuildStatus("Compiling", "compile", "pending");
                script {
                    try {
                        echo 'Deploying....'
                        setBuildStatus("Build complete", "compile", "success");
                    } catch (err) {
                        setBuildStatus("Failed", "pl-compile", "failure");
                        throw err
                    }
                }
            }
        }
    }
}
