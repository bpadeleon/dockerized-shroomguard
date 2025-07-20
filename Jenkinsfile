pipeline{
    agent any

    stages {
        stage('Build'){
            agent{
                // Initiate Build Environment
                docker{
                    image 'python:3.11-slim'
                    reuseNode true
                }
            }
            steps {
                // list directory contents
                sh 'ls -la'
                // Test application if its working and run
                sh 'pip install -r requirements.txt'
                sh 'pytest'
            }
        }

    }
}
