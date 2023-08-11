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
        stage('Test & Analyse') {
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
        stage('Deploy') {
                when(env.BRANCH_NAME != 'main'){

                }
                steps {
                    echo 'Deploying...'

                    script {
                        if (env.BRANCH_NAME == 'main') {
                            echo 'Main'
                        } else {
                            error "Unrecognized branch used!"
                        }
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
