@Library('shared-lib') _

pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhubpwd')
        DOCKERHUB_USERNAME = 'pramila188'
        SLACK_CREDENTIALS = 'slackpwd'
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                deleteDir() // Deletes all files in the workspace
            }
        }

        stage('Checkout Repositories') {
            parallel {
                stage('Checkout Java Application') {
                    steps {
                        git branch: 'main', url: 'https://github.com/pramilasawant/springboot1-application.git'
                    }
                }
                stage('Checkout Python Application') {
                    steps {
                        git branch: 'main', url: 'https://github.com/pramilasawant/phython-application.git'
                    }
                }
            }
        }

        stage('Build and Deploy') {
            parallel {
                stage('Build and Push Java Application') {
                    steps {
                        script {
                            docker.withRegistry('https://index.docker.io/v1/', 'dockerhubpwd') {
                                def javaImage = docker.build("${DOCKERHUB_USERNAME}/testhello:latest", 'Desktop/springboot1-application/testhello')
                                javaImage.push()
                            }
                        }
                    }
                }

                stage('Build and Push Python Application') {
                    steps {
                        script {
                            docker.withRegistry('https://index.docker.io/v1/', 'dockerhubpwd') {
                                def pythonImage = docker.build("${DOCKERHUB_USERNAME}/python-app:latest", 'Desktop/python-app')
                                pythonImage.push()
                            }
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs() // Clean workspace after build
        }
        success {
            slackSend (color: '#00FF00', message: "Build succeeded: ${env.JOB_NAME} ${env.BUILD_NUMBER}")
        }
        failure {
            slackSend (color: '#FF0000', message: "Build failed: ${env.JOB_NAME} ${env.BUILD_NUMBER}")
        }
    }
}
