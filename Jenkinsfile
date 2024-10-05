pipeline {
    agent any

    stages {
        stage('ECS') {
            
            agent {
                docker {
                    image 'amazon/aws-cli'
                    args "--entrypoint=''"
                    reuseNode true
                }
            }
            steps {
                
            withCredentials([usernamePassword(credentialsId: 'AWS', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
            
            sh '''

                aws ecs register-task-definition --cli-input-json AWS/task-defination-prod.json
                '''

}
            }
        }
    }
}
