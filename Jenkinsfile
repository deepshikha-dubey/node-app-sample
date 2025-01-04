pipeline{
    agent {
        label 'worker'
    }
    stages{
        stage("Build and Push"){
            steps{
                sh '''
                     aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 509399595711.dkr.ecr.us-east-1.amazonaws.com
                     docker build -t 509399595711.dkr.ecr.us-east-1.amazonaws.com/my-ecr:nodeimage .
                     docker push 509399595711.dkr.ecr.us-east-1.amazonaws.com/my-ecr:nodeimage
                '''
            }
        }        
    }
}
