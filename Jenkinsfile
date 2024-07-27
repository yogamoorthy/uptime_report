pipeline {
    agent any
    stages {
        stage('Setup') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'SYSTEMS_VAULT_APPROLE', 
                    passwordVariable: 'VAULT_SECRET_ID', 
                    usernameVariable: 'VAULT_APPROLE_ID')]
                ) {
                sh """
                set +x
                ./do/vault ${VAULT_APPROLE_ID} ${VAULT_SECRET_ID}
                ./do/setup
                """
                }
            }
        }
        stage('Run') {
            parallel {
                stage('Run') {
                    steps {
                        sh script: """
                        . ./venv/bin/activate
                        ./do/run
                        """
                    }
                }
            }
        }
        stage('Cleanup') {
            steps {
            sh './do/cleanup'
            }
        }
    }
    post {
        cleanup {
            cleanWs()
        }
    }
}
