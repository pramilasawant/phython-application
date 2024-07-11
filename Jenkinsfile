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

        stage('Build and Deploy') {
            steps {
                script {
                    // Calling a custom build step defined in shared library
                    build(
                        javaRepo: 'https://github.com/pramilasawant/springboot1-application.git',
                        pythonRepo: 'https://github.com/pramilasawant/phython-application.git'
                    )
                }
            }
        }
    }
    
    post {
        always {
            // Clean up any resources if needed
        }
        success {
            // Actions to take if the pipeline succeeds
            echo "Pipeline completed successfully!"
        }
        failure {
            // Actions to take if the pipeline fails
            echo "Pipeline failed!"
        }
    }
}
