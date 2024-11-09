pipeline {
    agent any

    tools {
        nodejs "Nodes" // Ensure that this matches the NodeJS name in Jenkins Global Tool Configuration
    }

    stages {
        stage('Install Dependencies') {
            steps {
                // Install npm packages
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                // Example: compile frontend assets if necessary
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                // Run your tests
                sh 'npm test'
            }
        }

        stage('Deploy to Server') {
            steps {
                // SSH into the server and deploy
                sshagent(credentials: ['02afc25f-95f8-41cf-a873-2faa6e8f7dfe']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@65.2.189.80 << EOF
                    cd /path/to/deployment
                    git pull origin main
                    npm install --production
                    pm2 restart all
                    EOF
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up and notifying...'
            // Optionally, add clean-up steps or notifications (email, slack, etc.)
        }

        success {
            echo 'Deployment completed successfully!'
            // Optionally, notify that the build was successful (e.g., via Slack, email, etc.)
        }

        failure {
            echo 'Deployment failed!'
            // Optionally, notify failure (e.g., via Slack, email, etc.)
        }
    }
}
