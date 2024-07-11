pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhubpwd')
    }
    stages {
        stage('Cleanup Workspace') {
            steps {
                deleteDir()
            }
        }
        stage('Checkout Repositories') {
            parallel {
                stage('Checkout Java Application') {
                    steps {
                        dir('java-app') {
                            git 'https://github.com/pramilasawant/springboot1-application.git'
                        }
                    }
                }
                stage('Checkout Python Application') {
                    steps {
                        dir('python-app') {
                            git 'https://github.com/pramilasawant/phython-application.git'
                        }
                    }
                }
            }
        }
        stage('Build and Deploy') {
            parallel {
                stage('Build and Push Java Application') {
                    steps {
                        script {
                            dir('java-app') {
                                sh 'mvn clean package'
                                docker.withRegistry('https://index.docker.io/v1/', 'dockerhubpwd') {
                                    def app = docker.build("pramila188/testhello:latest")
                                    app.push()
                                }
                            }
                        }
                    }
                }
                stage('Build and Push Python Application') {
                    steps {
                        script {
                            dir('python-app') {
                                docker.withRegistry('https://index.docker.io/v1/', 'dockerhubpwd') {
                                    def app = docker.build("pramila188/python-app:latest")
                                    app.push()
                                }
                            }
                        }
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs()
            slackSend(color: '#FF0000', message: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL}) failed.")
        }
    }
}
