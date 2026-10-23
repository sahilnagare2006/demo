pipeline {
    agent any

    environment {
        PATH = "/opt/homebrew/bin:/usr/local/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                sh 'pwd && ls -la'
            }
        }

        stage('Detect Projects') {
            steps {
                script {
                    def dirs = sh(
                        script: "find . -name package.json -not -path '*/node_modules/*' -exec dirname {} \\;",
                        returnStdout: true
                    ).trim()

                    if (!dirs) {
                        error 'No package.json found in the repo. Check your branch and repo contents.'
                    }
                    env.APP_DIRS = dirs
                    echo "Projects found:\n${dirs}"
                }
            }
        }

        stage('Install') {
            steps {
                script {
                    for (d in env.APP_DIRS.split('\n')) {
                        dir(d) { sh 'npm install' }
                    }
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    for (d in env.APP_DIRS.split('\n')) {
                        dir(d) { sh 'npm run build --if-present' }
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    for (d in env.APP_DIRS.split('\n')) {
                        dir(d) { sh 'npm test --if-present' }
                    }
                }
            }
        }
    }
}
