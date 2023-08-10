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
        stage('Checkout') {
            steps {
                echo 'Checkout.. ' + env.BRANCH_NAME
                checkout scm
            }
        }
        stage('Test & Analyse - check') {
            steps {
            echo 'Test & Analyse'
                withGradle {
                    sh 'gradle check'
                }
            }
        }
        stage('Build') {
            steps {
                echo 'Build'
                withGradle {
                    sh 'gradle build'
                  }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
                withGradle {
                    sh 'gradle test'
                  }
            }
        }
        stage('Deploy') {
        when { equals expected: true, actual: Deploy }
            steps {
                echo 'Deploying...'
                script {

                }
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
