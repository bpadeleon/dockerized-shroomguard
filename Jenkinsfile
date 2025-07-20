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
            }
        }
        stage('Test'){
            steps{ 
                // Test Application if its running
                sh 'python mushroom.py'
                sleep time: 5, unit: 'SECONDS'
                sh 'echo "Application is running successfully"'
            }
        }
        stage('Update Dockerfile') {
            steps {
                script {
                    sh '''
                        git add Dockerfile
                        git diff --cached --quiet || git commit -m "chore(ci): update Dockerfile after successful test"
                        git push origin HEAD:main
                    '''
                }
            }
        }
        stage('Deploy'){
            steps{
                sh 'docker build -t mushroom-app:latest .'
                sh 'docker run -d -p 5000:5000 --name mushroom-app mushroom-app:latest'
            }

        }

    }
}