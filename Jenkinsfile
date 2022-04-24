pipeline {
    agent { label 'main' }

    environment {
        BUILD_NUM = "${currentBuild.number}"
        WORKSPACE = "${env.WORKSPACE}"
        BRANCH = "${env.BRANCH_NAME}"
    }

    options {
        skipDefaultCheckout true
    }

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

        stage('Publish'){
            steps {
                script{
                    if ("${BRANCH}" == "main"){
                    echo "Publishing artifact to S3"
                    sh "zip -r rdicidr-${BUILD_NUM}.zip build"
                    //s3Upload(sourceFile: "${WORKSPACE}/rdicidr-${BUILD_NUM}.zip", bucket: 'fsl-artifacts', path: 'rdicidr/artifacts') 
                    }
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