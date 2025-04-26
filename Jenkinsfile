pipeline {
    agent any

    environment {
        AZURE_USER = 'azureuser'               // Your Azure VM username
        AZURE_IP = '20.169.202.24'             // Your Azure VM IP address
        KALI_USER = 'giridhar'                 // Your Kali VM username
        KALI_IP = '192.168.56.104'             // Your Kali VM IP address
    }

    stages {
        stage('Checkout SCM') {
            steps {
                // Checkout code from the repository
                checkout scm
            }
        }

        stage('Start Splunk Enterprise on Azure') {
            steps {
                echo "Skipping automatic start of Splunk on Azure."
                // You can manually start Docker containers here on Azure.
                // Uncomment this line if you want Jenkins to start it automatically:
                // sshagent(['azure-ssh-credentials']) {
                //     sh 'ssh -o StrictHostKeyChecking=no $AZURE_USER@$AZURE_IP "cd /home/azureuser/docker-log-monitoring && docker-compose up -d"'
                // }
            }
        }

        stage('Start Universal Forwarder on Kali') {
            steps {
                echo "Skipping automatic start of Universal Forwarder on Kali."
                // You can manually start Docker containers here on Kali.
                // Uncomment this line if you want Jenkins to start it automatically:
                // sshagent(['kali-ssh-credentials']) {
                //     sh 'ssh -o StrictHostKeyChecking=no $KALI_USER@$KALI_IP "cd /home/giridhar/devops/SplunkUniversal && docker-compose up -d"'
                // }
            }
        }
    }

    post {
        success {
            echo '✅ Build completed successfully! You can now manually start and stop the containers.'
        }
        failure {
            echo '❌ Something went wrong during the build.'
        }
    }
}
