pipeline {
    agent any

    environment {
        AZURE_USER = 'azureuser'              // Azure VM username
        AZURE_IP = '20.169.202.24'            // Azure VM IP
        KALI_USER = 'giridhar'                // Kali VM username
        KALI_IP = '192.168.56.104'            // Kali VM IP
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/giriruppa/log-monitoring-docker-splunk'
            }
        }

        stage('Start Splunk Enterprise on Azure') {
            steps {
                sshagent(['azure-ssh-credentials']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no $AZURE_USER@$AZURE_IP "\
                            cd ~/docker-log-monitoring/splunk-enterprise && \
                            docker-compose up -d"
                    '''
                }
            }
        }

        stage('Start Universal Forwarder on Kali') {
            steps {
                sshagent(['kali-ssh-credentials']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no $KALI_USER@$KALI_IP "\
                            cd ~/SplunkUniversal && \
                            docker-compose up -d"
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
