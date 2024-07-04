pipeline {
    agent any

    environment {
        ANSIBLE_HOSTS = "hosts.ini"  // Path to your Ansible hosts file
        CREDENTIAL_ID = 'connectioncred'  // Jenkins credential ID for Ansible
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Checkout the Git repository where your Ansible playbook and inventory are stored
                git 'https://github.com/vaibhavkalel1/Windows-Ansible-Connection.git'
            }
        }

        stage('Configure WinRM on Windows Server') {
            steps {
                script {
                    // Run Ansible playbook to configure WinRM
                    ansiblePlaybook(
                        playbook: 'connection_playbook.yml',
                        inventory: ANSIBLE_HOSTS,
                        credentialsId: CREDENTIAL_ID,  // Use Jenkins credential ID for Ansible
                        extraVars: [
                            // Define any extra variables needed
                        ]
                    )
                }
            }
        }

        stage('Check Connection') {
            steps {
                // Check connection to Windows hosts using Ansible win_ping module
                sh "ansible -i ${ANSIBLE_HOSTS} windows -m win_ping"
            }
        }
    }

    post {
        always {
            // Clean up steps if necessary
            echo "Pipeline execution completed"  // Example of a cleanup step (can be replaced with actual cleanup tasks)
        }
    }
}
