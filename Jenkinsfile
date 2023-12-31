pipeline {
    agent any
    environment {
        CI = 'true'
        CURR_DIR = pwd()
        API_DIR = '/swaggerpets'
    }
    stages {
        stage('Preparation') {
            steps {
                git branch: "main",
                        url: 'https://github.com/islom-kh/swaggerpets.git',
                        credentialsId: 'amp-demo-github-cred'
            }
        }

        stage('Deploy to Dev') {
            environment {
                RETRY = '80'
            }
            steps {
                echo 'Logging into $DEV_ENV'
                sh 'apictl login dev -u admin -p admin -k'

                echo 'Deploying to $DEV_ENV'
                sh 'apictl import-api -f $CURR_DIR$API_DIR -e dev -k --preserve-provider --update --verbose'
            }
        }
    }
    post {
        cleanup {
            deleteDir()
            dir("${workspace}@tmp") {
                deleteDir()
            }
            dir("${workspace}@script") {
                deleteDir()
            }
        }
    }
}
