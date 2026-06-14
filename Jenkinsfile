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
        timeout(time: 10, unit: 'SECONDS')
        disableConcurrentBuilds()
    }

    // This is build section information for reference
    stages {
        stage('Read Version') {
            steps {
                script {
                    def packageJSON = readJSON file: 'package.json'
                    appVersion = packageJSON.version
                    echo "app version: ${appVersion}"
                }
            }
        }

        // This is Test section information for reference
        stage('Test') {
            steps {
                echo "Testing"
            }
        }

        // This is deploy section information for reference
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
                expression { "$params.DEPLOY" == "true" }
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

