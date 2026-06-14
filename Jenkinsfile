pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }

    environment {
        COURSE = "Jenkins"
        appVersion = ""
    }

    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
    }

    // This is build section information for reference
    stages {
        stage('Read Version') {
            steps {
                script {
                    def packageJSON = readJSON file: 'package.json'
                    env.appVersion = packageJSON.version
                    echo "app version: ${env.appVersion}"
                }
            }
        }

        // This is Test section information for reference
        stage('Install Depedencies') {
            steps {
                script {
                    sh """
                        npm install
                    """
                }
            }
        }

        stage('Build Image') {
            steps {
                script {
                    sh """
                        docker build -t catalogue:${env.appVersion} .
                        docker images
                    """
                }
            }
        }

        stage('Deploy') {

            // input {
            //     message "Should we continue?"
            //     ok "Yes, we should."
            //     submitter "alice,bob"

            //     parameters {
            //         string(
            //             name: 'PERSON',
            //             defaultValue: 'Mr Jenkins',
            //             description: 'Who should I say hello to?'
            //         )
            //     }
            // }

            when {
                expression { params.DEPLOY == true }
            }

            steps {
                echo "Deploying"
            }
        }
    }

    post {
        always {
            echo 'I will always say Hello again!'
            cleanWs()
        }

        success {
            echo 'I will run if success'
        }

        failure {
            echo 'I will run if failure'
        }
    }
}