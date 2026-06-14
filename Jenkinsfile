pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }

    parameters {
        booleanParam(
            name: 'DEPLOY',
            defaultValue: false,
            description: 'Deploy application'
        )
    }

    environment {
        COURSE = "Jenkins"
        ACC_ID = "074654842955"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
    }

    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
    }

    stages {

        stage('Read Version') {
            steps {
                script {
                    def packageJSON = readJSON file: 'package.json'
                    env.appVersion = packageJSON['version']   
                    echo "Application Version: ${env.appVersion}"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    withAWS(region: 'us-east-1', credentials: 'aws-creds') {
                        sh """
                            aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${env.ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                            docker build -t ${env.ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${env.PROJECT}/${env.COMPONENT}:${env.appVersion} .
                            docker images
                            docker push ${env.ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${env.PROJECT}/${env.COMPONENT}:${env.appVersion}
                        """
                    }
                }
            }
        }

        stage('Deploy') {
            when {
                expression {
                    return params.DEPLOY
                }
            }
            steps {
                echo "Deploying catalogue:${env.appVersion}"
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed'
            cleanWs()
        }
        success {
            echo 'Build completed successfully'
        }
        failure {
            echo 'Build failed'
        }
    }
}