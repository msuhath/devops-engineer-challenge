pipeline {
    agent any

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Downloading source code from GitHub'

                git branch: 'master',
                    url: 'https://github.com/riyanuddin17/DevOps-Challenge.git'
            }
        }

        stage('Check Tools') {
            steps {
                sh '''
                    set -e

                    echo "Node.js version:"
                    node -v

                    echo "npm version:"
                    npm -v

                    echo "Git version:"
                    git --version

                    echo "AWS CLI version:"
                    aws --version
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    set -e
                    npm install
                '''
            }
        }

        stage('Build React App') {
            steps {
                sh '''
                    set -e

                    npm run build

                    test -f dist/index.html

                    echo "React build completed successfully"
                '''
            }
        }

        stage('Prepare Deployment') {
            steps {
                sh '''
                    set -e

                    echo "Preparing deployment files"

                    rm -rf /tmp/react-dist
                    mkdir -p /tmp/react-dist

                    cp -r dist/. /tmp/react-dist/

                    test -f /tmp/react-dist/index.html

                    echo "Deployment files prepared successfully"
                '''
            }
        }

        stage('Deploy with PM2') {
            steps {
                sh '''
                    set -e

                    echo "Deploying application with PM2"

                    sudo -n /usr/local/bin/deploy-react-app
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    set -e

                    echo "Checking application on port 3000"

                    for i in $(seq 1 10); do
                        if curl -fsS http://127.0.0.1:3000/ -o /dev/null; then
                            echo "SUCCESS: Application is responding"
                            exit 0
                        fi

                        echo "Attempt $i failed; waiting 2 seconds"
                        sleep 2
                    done

                    echo "ERROR: Application did not respond"
                    exit 1
                '''
            }
        }

        stage('Upload Build to S3') {
            steps {
                sh '''
                    set -e

                    echo "Uploading React build files to S3"

                    test -f dist/index.html

                    aws s3 sync dist/ \
                        s3://mesba-devops-challenge-2026/

                    echo "SUCCESS: React build uploaded to S3"
                '''
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Build, deployment and S3 upload completed'
        }

        failure {
            echo 'FAILURE: Check Console Output for the failed stage'
        }

        always {
            echo 'Jenkins pipeline execution finished'
        }
    }
}

