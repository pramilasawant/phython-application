@Library('shared-lib') _

pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhunpwd')
        DOCKERHUB_USERNAME = 'pramila188'
        SLACK_CREDENTIALS = 'slackpwd'
    }
    stages {
        stage('Cleanup Workspace') {
            steps {
                deleteDir() // Deletes all files in the workspace
            }
        }
        stage('Build and Deploy') {
            steps {
                script {
                    build(
                        javaRepo: 'https://github.com/pramilasawant/springboot1-application.git',
                        pythonRepo: 'https://github.com/pramilasawant/phython-application.git'
                    )
                }
            }
        }
    }
}
