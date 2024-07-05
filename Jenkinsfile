pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/mouryasimhareddy/Jenkins'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Deploy') {
            steps {
                withCredentials([sshUserPrivateKey(
                    credentialsId: 'your-ssh-credential-id',
                    keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                        scp -i $SSH_KEY -r build/* ec2-user@your-ec2-instance:/var/www/html
                        ssh -i $SSH_KEY ec2-user@your-ec2-instance 'sudo systemctl restart httpd'
                    '''
                }
                // Commands to deploy your build, e.g., copying files to a server
            }
        }
    }
}
