pipeline {
    agent any {
        parameters { choice (name : 'ENVIRONMENT',
                             choices : ['dev', 'staging', 'production']) }
    stages {
        stage('Approve') {
            when { expression { params.ENVIRONMENT == 'production' } }
            step { input message: 'Deploy to production ? ' }
        }
        stage('Deploy') {
            steps { bat "echo Deploy to ${params.ENVIRONMENT}" }
        }
    }
}

