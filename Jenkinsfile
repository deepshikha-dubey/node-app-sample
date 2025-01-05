pipeline{
    agent {
        label 'worker'
    }
    stages{
        stage("Build and Push"){
            steps{
                sh '''
                     aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 509399595711.dkr.ecr.us-east-1.amazonaws.com
                     docker build -t 509399595711.dkr.ecr.us-east-1.amazonaws.com/node-app-repo:node-app .
                     docker push 509399595711.dkr.ecr.us-east-1.amazonaws.com/node-app-repo:node-app
                '''
            }
        }  

        stage("Run"){
            steps{
                sh '''
                     docker run -d -p 3000:3000 --name nodeapp 509399595711.dkr.ecr.us-east-1.amazonaws.com/node-app-repo:node-app
                '''
            }
        }      
    }
}

pipeline {
    agent 
     {
        label 'jenkins'
    }
    environment {
        DOCKER_IMAGE = "node-app-sample"
        REPO_URL = "https://github.com/deepshikha-dubey/example-voting-app.git"
        ECR_REPO = "509399595711.dkr.ecr.us-east-1.amazonaws.com"
        AWS_REGION = "us-east-1"
        AWS_ACCESS_KEY_ID = credentials('AKIAXIHXMA6TMWSTZU6K')
        AWS_SECRET_ACCESS_KEY = credentials('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')
    }

    stages {
        stage('Checkout') {
            steps {
                git url: "${REPO_URL}", branch: 'main'
            }
        }
        stage('Build and Push Docker Image') {
            steps {
                script {
                    sh """
                    aws configure set aws_access_key_id ${AWS_ACCESS_KEY_ID}
                    aws configure set aws_secret_access_key ${AWS_SECRET_ACCESS_KEY}
                    aws configure set region ${AWS_REGION}

                    $(aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REPO})
                    docker build -t ${ECR_REPO}/${DOCKER_IMAGE}:vote-app .
                    docker push ${ECR_REPO}/${DOCKER_IMAGE}:vote-app
                    """
                }
            }
        }
    }
}
