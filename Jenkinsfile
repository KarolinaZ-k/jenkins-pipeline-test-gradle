pipeline {
    agent any

    tools {
            gradle '8.2.1'
        }

    options {
            timeout(time: 10, unit: 'MINUTES')
            disableConcurrentBuilds()
        }

    stages {
        stage('Test & Analyse') {
            steps {
                withGradle {
                    sh 'gradle check --continue'
                }
            }
        }
        stage('Build') {
            steps {
                echo 'Build'
                withGradle {
                    bat 'gradle build'
                  }
            }
        }
        stage('Testsssss') {
                    steps {
                        echo 'Testing...'
                        withGradle {
                            bat 'gradle test'
                          }
                    }
                }
        stage('Test') {
            steps {
                echo '"Fail!"; exit 1'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploy'
            }
        }
    }
    post {
        always {
            echo 'always'
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
