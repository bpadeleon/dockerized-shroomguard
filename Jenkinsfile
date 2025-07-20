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
                sh 'echo "Application is running successfully"'
            }
        }

        stage('Update Dockerfile') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github_creds', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                    sh '''
                        git config user.name "jenkins-bot"
                        git config user.email "jenkins@example.com"
                        git remote set-url origin https://${GIT_USER}:${GIT_PASS}@github.com/bpadeleon/dockerized-shroomguard.git

                        git add .
                        if ! git diff --cached --quiet; then
                            git commit -m "chore(ci): update codebase after successful test"
                            git push origin HEAD:main
                        else
                            echo "No changes to commit"
                        fi
                    '''
                }
            }
        }
        stage('Deploy'){
            steps{
                sh 'docker build --network=host --dns=8.8.8.8 -t mushroom-app:latest .'
                sh 'docker run -d -p 5000:5000 --name mushroom-app mushroom-app:latest'
            }

        }

    }
}