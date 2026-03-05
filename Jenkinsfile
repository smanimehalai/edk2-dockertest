pipeline {

    agent {
        docker {
            image 'edk2-aadp:1.0'
            args '-v /edk2-cache:/edk2-cache'
        }
    }

    options {
        disableConcurrentBuilds()
        timestamps()
    }

    stages {

        stage('Checkout Source') {
            steps {
                checkout scm
            }
        }

        stage('Configure Git') {
            steps {
                sh '''
                git config --global user.email "manimehalai.s@adlinktech.com"
                git config --global user.name "manimehalai.s"
                '''
            }
        }

        stage('Build') {
            steps {
                sh 'bash make_ComHpcAlt.sh'
            }
        }
    }
}
