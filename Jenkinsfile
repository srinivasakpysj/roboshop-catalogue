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
    appVersion = ""
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
                env.appVersion = packageJSON.version
                echo "Application Version: ${env.appVersion}"
            }
        }
    }

    stage('Install Dependencies') {
        steps {
            sh 'npm install'
        }
    }

    stage('Debug Workspace') {
        steps {
            sh '''
                echo "Current Directory:"
                pwd

                echo "Workspace Files:"
                ls -ltr
            '''
        }
    }

    stage('Build Image') {
        steps {
            sh """
                docker build -t catalogue:${env.appVersion} .
                docker images
            """
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
