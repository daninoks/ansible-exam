properties([disableConcurrentBuilds()])

pipeline {
    agent {
        label 'main-slave'
    }
    options {
        timestamps()
    }
    stages {
        stage("Deploy") {
            steps {
                sh """
                mkdir -p roles
                ansible-galaxy install -p roles -r requirements.yml
                """
                ansiblePlaybook(playbook: 'playbook.yml', inventory: 'inventory', vaultCredentialsId: 'vault-key-file', disableHostKeyChecking: true,)
            }         
        }
        stage("Test") {
            steps {
                ansiblePlaybook(playbook: 'test.yml', inventory: 'inventory')
            }          
        }        
    }
}
