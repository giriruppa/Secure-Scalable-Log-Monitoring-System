pipeline {
    agent any

    environment {
        AZURE_USER = 'azureuser'               // Change to your actual Azure VM username
        AZURE_IP = '20.169.202.24'             // Your Azure VM IP address
        KALI_USER = 'giridhar'                     // Change to your Kali VM username
        KALI_IP = '192.168.56.104'               // Your Kali VM IP address
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/giriruppa/log-monitoring-docker-splunk'
            }
        }

        stage('Start Splunk Enterprise on Azure') {
            steps {
                sshagent(['azure-ssh-credentials']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no $AZURE_USER@$AZURE_IP "cd ~/home/azureuser/docker-log-monitoring && docker-compose up -d"
                    '''
                }
            }
        }

        stage('Start Universal Forwarder on Kali') {
            steps {
                sshagent(['kali-ssh-credentials']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no $KALI_USER@$KALI_IP "cd ~/home/giridhar/devops/SplunkUniversal && docker-compose up -d"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Splunk Enterprise and Universal Forwarder started successfully!'
        }
        failure {
            echo '❌ Something went wrong while deploying containers.'
        }
    }
}
