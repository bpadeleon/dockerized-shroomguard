pipeline{
    agent any

    stages {
        stage('Build'){
            agent{
                // Initiate Build Environment
                docker{
                    image 'python:3.11-slim'
                    reuseNode true
                    args '--network=host'
                }
            }
            steps {
                // list directory contents
                sh 'ls -la'
                sh 'mkdir -p /tmp/pip-tmp'
                // Test application if its working and run
                sh 'pip install --no-cache-dir --upgrade -r requirements.txt'
                sh 'python mushroom.py'
            }
        }

    }
}
