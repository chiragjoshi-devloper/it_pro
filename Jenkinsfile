pipeline {
    agent any

    tools {
        nodejs "Your_NodeJS_Environment_Name"
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
                sshagent (credentials: ['your-ssh-credential-id']) {
                    sh '''
                    ssh user@your-server-ip << EOF
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
            echo 'Clean up!'
            // Optionally clean up or notify
        }
    }
}
