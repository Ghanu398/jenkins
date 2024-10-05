pipeline {
    agent any
    environment {
       AWS_DEFAULT_REGION = 'us-east-1'
    }
    stages {
        stage('ECS') {
            
            agent {
                docker {
                    image 'amazon/aws-cli'
                    args "-u root --entrypoint=''"
                    reuseNode true
                }
            }
            steps {
                
            withCredentials([usernamePassword(credentialsId: 'AWS', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
            
            sh '''
                
                aws ecs register-task-definition --cli-input-json file://AWS/task-defination-prod.json > output-file.json
                #update -y
                yum install jq -y
                VERSION=$(jq '.taskDefinition.taskDefinitionArn' output-file.json | awk -F ':' '{print $NF}')
                
                echo "$VERSION"
                '''

}
            }
        }
    }
}
