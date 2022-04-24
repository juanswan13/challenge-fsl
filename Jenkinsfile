pipeline {
    agent { label 'main' }

    stages{
        stage('Checkout'){
            steps {    
                cleanWs()
                checkout scm
            }
        }

        stage('NPM Install'){
            steps {
                nodejs(nodeJSInstallationName: 'Node 12.22.12') {
                    sh "npm install"
                }
            }
        }

        stage('Lint'){
            steps {
                nodejs(nodeJSInstallationName: 'Node 12.22.12') {
                    sh "npm run lint"
                }
            }
        }

        stage('Formatter'){
            steps {
                nodejs(nodeJSInstallationName: 'Node 12.22.12') {
                    sh "npm run prettier"
                }
            }
        }

        stage('Test'){
            steps {
                nodejs(nodeJSInstallationName: 'Node 12.22.12') {
                    sh "CI=true npm run test"
                }
            }
        }

        stage('Build'){
            steps {
                nodejs(nodeJSInstallationName: 'Node 12.22.12') {
                    sh "npm run build"
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}