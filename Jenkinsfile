pipeline {
    agent any

    stages {

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
        
        stage('Deploy') {
            steps {
                withCredentials([sshUserPrivateKey(
                    credentialsId: 'counter-app-server-ec2',
                    keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                        mkdir -p ~/.ssh
                        ssh-keyscan -H 172.31.17.79 >> ~/.ssh/known_hosts 
                        scp -i $SSH_KEY -r build/* ec2-user@172.31.17.79:/var/www/html
                        ssh -i $SSH_KEY ec2-user@172.31.17.79 'sudo systemctl restart httpd'
                    '''
                }
            }
        }
    }
}
