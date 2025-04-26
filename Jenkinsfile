pipeline {
    agent any

    environment {
        AZURE_USER = 'azureuser'               // Your Azure VM username
        AZURE_IP = '20.169.202.24'             // Your Azure VM IP address
        KALI_USER = 'giridhar'                 // Your Kali VM username
        KALI_IP = '192.168.56.104'             // Your Kali VM IP address
    }

    stages {
        stage('Prepare Infrastructure') {
            steps {
                echo '✅ Build Success - Containers not started automatically. Please start and stop containers manually.'
            }
        }

        stage('Start Splunk Enterprise on Azure (Manual)') {
            steps {
                echo 'To start Splunk Enterprise on Azure, run the following command manually:'
                echo 'ssh -o StrictHostKeyChecking=no $AZURE_USER@$AZURE_IP "cd /home/azureuser/docker-log-monitoring && docker-compose up -d"'
            }
        }

        stage('Start Universal Forwarder on Kali (Manual)') {
            steps {
                echo 'To start Universal Forwarder on Kali, run the following command manually:'
                echo 'ssh -o StrictHostKeyChecking=no $KALI_USER@$KALI_IP "cd /home/giridhar/devops/SplunkUniversal && docker-compose up -d"'
            }
        }
    }

    post {
        success {
            echo '✅ Jenkins build completed successfully! You can now manually manage your containers.'
        }
        failure {
            echo '❌ Something went wrong during the build process.'
        }
    }
}
