pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                bat 'set'
            }
        }
        stage('Test') {
            steps {
                echo '"Fail!"; exit 1'
            }
        }
        stage('Deploy') {
            steps {
                retry(1) {
                    echo 'Deploy pary retry 1 times"'
                }

                timeout(time: 1, unit: 'MINUTES') {
                    echo 'timeout 1 minute'
                }
            }
        }
    }
    post {
            always {
                junit 'build/reports/**/*.xml'
            }
            success {
                echo 'This will run only if successful'
            }
            failure {
                echo 'This will run only if failed'
            }
            unstable {
                echo 'This will run only if the run was marked as unstable'
            }
            changed {
                echo 'This will run only if the state of the Pipeline has changed'
                echo 'For example, if the Pipeline was previously failing but is now successful'
            }
        }
}
