pipeline {
    agent any

    tools {
            gradle '8.2.1'
            jfrog '2.30.0'
        }

    options {
            timeout(time: 10, unit: 'MINUTES')
            disableConcurrentBuilds()
        }

    stages {
        stage('Main') {
            agent {
                label env.AGENT_TYPE
            }
            options {
                timeout(time: 30, unit: 'MINUTES')
            }
            environment {
                DOCKERHUB_ORGANISATION = "${infra.isTrusted() ? 'jenkins' : 'jenkins4eval'}"
            }
            stages {
                stage('Prepare Docker') {
                    when {
                        environment name: 'AGENT_TYPE', value: 'linux'
                    }
                    steps {
                        sh '''
                        docker buildx create --use
                        docker run --rm --privileged multiarch/qemu-user-static --reset -p yes
                        '''
                    }
                }
            }
        }
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
                    if (env.BRANCH_NAME == 'main') {
                        sh "cp app/build/libs/fat-jar.jar app/build/libs/latest.jar"
                        jf "rt u app/build/libs/fat-jar.jar jenkins-pipeline-test-gradle/Development/latest/master.jar"
                    } else {
                        error "Unrecognized branch used!"
                    }
                }
            }
        }
    }
    post {
        always {
            junit '**/build/test-results/**/*.xml'
            archiveArtifacts artifacts: "app/build/distributions/jenkins-pipeline-test-gradle-${env.BUILD_NUMBER}.zip", fingerprint: true
            deleteDir()
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
    }
}
