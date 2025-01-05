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
