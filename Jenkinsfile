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
        steps {
                        echo 'Deploying...'
                        script {
                            if (env.BRANCH_NAME == 'master') {
                                sh "cp app/build/libs/fat-jar.jar app/build/libs/latest.jar"
                                jf "rt u app/build/libs/fat-jar.jar maght-generic-repository/Development/latest/master.jar"
                            } else if ((env.BRANCH_NAME).startsWith('feature/')) {
                                def artifactName = env.BRANCH_NAME.substring(env.BRANCH_NAME.lastIndexOf('/') + 1, env.BRANCH_NAME.length())
                                sh "cp app/build/libs/fat-jar.jar app/build/libs/${artifactName}.jar"
                                jf "rt u app/build/libs/${artifactName}.jar maght-generic-repository/Development/features/${artifactName}.jar"
                            } else if ((env.BRANCH_NAME).startsWith('release/')) {
                                error "Release deploy not configured yet!"
                            } else if ((env.BRANCH_NAME).startsWith('bugfix/')) {
                                def artifactName = env.BRANCH_NAME.substring(env.BRANCH_NAME.lastIndexOf('/') + 1, env.BRANCH_NAME.length())
                                sh "cp app/build/libs/fat-jar.jar app/build/libs/${artifactName}.jar"
                                jf "rt u app/build/libs/${artifactName}.jar maght-generic-repository/Development/bugfixes/${artifactName}.jar"
                            } else {
                                error "Unrecognized branch used!"
                            }
                        }
                    }
            when {
                expression {
                    return true
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
