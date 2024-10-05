pipeline {
    agent any
    environment {
       AWS_DEFAULT_REGION = 'us-east-1'
       AWS_SERVICE = 'learn-docker'
       AWS_CLUSTER = 'test-cluster'
       AWS_TD = 'test-task-defination'

    }
    stages {

        stage("image build"){
            steps {
                sh '''
                docker image build -t 'aws-cli-manual' .
                '''
            }
        }
    
        stage('ECS') {
            
            agent {
                docker {
                    image 'aws-cli-manual'
                    //args "-u root --entrypoint=''"
                    reuseNode true
                }
            }
            steps {
                
            withCredentials([usernamePassword(credentialsId: 'AWS', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
            
            sh '''
                
                aws ecs register-task-definition --cli-input-json file://AWS/task-defination-prod.json > output-file.json
                #update -y
                #yum install jq -y
                VERSION=$(jq '.taskDefinition.taskDefinitionArn' output-file.json | awk -F ':' '{print $NF}' | awk -F '"' '{print $1}')
                aws ecs update-service --cluster $AWS_CLUSTER --service $AWS_SERVICE --task-definition $AWS_TD:$VERSION
                aws ecs wait services-stable --cluster $AWS_CLUSTER --services $AWS_SERVICE

                '''


            }
        }
    }
}
}
