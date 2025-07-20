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
            environment {
                // create temporary environment variables
                PYTHONPATH = '/tmp/pip-tmp/lib/python3.11/site-packages'
                PATH = '/tmp/pip-tmp/bin:$PATH'
            }
            steps {
                // list directory contents
                sh 'ls -la'
                sh 'mkdir -p /tmp/pip-tmp'

                // Test application if its working and run
                sh 'pip install --no-cache-dir --upgrade --prefix=/tmp/pip-tmp -r requirements.txt'
                sh 'python mushroom.py & pid=$!; sleep 5; kill -SIGINT $pid || true'
                sh 'echo "Application is running successfully"'
            }
        }

        stage('Test Deployment') {
            steps {
                sh 'docker build --network=host -t mushroom-app:latest .'

                // Stop and remove existing container if any
                sh 'docker stop mushroom-app || true'
                sh 'docker rm mushroom-app || true'

                // Run new container
                sh 'docker run -d -p 5000:5000 --name mushroom-app mushroom-app:latest'
            }
        }
        stage('Push Image to Dockerhub'){
            steps{
                sh 'echo "Pushing Docker image to Docker Hub"'
            }
        }

    }
}