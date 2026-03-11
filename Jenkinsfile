pipeline {

    agent {
        docker {
            image 'edk2-aadp:1.0'
            args '-v /edk2-cache:/edk2-cache'
        }
    }

    environment {
        BUILD_OUTPUT_DIR = "${WORKSPACE}"
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

        stage('Update Submodules') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'github-edkii',
                    usernameVariable: 'GIT_USER',
                    passwordVariable: 'GIT_TOKEN'
                )]) {
                    sh '''
                    git config --global url."https://${GIT_USER}:${GIT_TOKEN}@github.com/".insteadOf "https://github.com/"

                    cd ${BUILD_OUTPUT_DIR}

                    # this script handles all submodule updates
                    bash setup_git.sh
                    '''
                }
            }
        }

        stage('Build EDK2') {
            steps {
                sh '''
                cd ${BUILD_OUTPUT_DIR}
                bash make_ComHpcAlt.sh
                '''
            }
        }
    }
}
