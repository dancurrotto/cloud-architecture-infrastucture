pipeline {
    agent any 
    
     environment {
                WORKSPACE=pwd()
                ETAG=''
    }
    stages {
        stage('Build'){            
            steps{
               
                sh 'echo building...'
                // git url: 'https://github.com/dancurrotto/cloud-architecture-infrastucture.git'
            }
        }
        stage('Deploy') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    // available as an env variable, but will be masked if you try to print it out any which way
                    // note: single quotes prevent Groovy interpolation; expansion is by Bourne Shell, which is what you want
                    sh 'echo $PASSWORD'
                    // also available as a Groovy variable
                    echo USERNAME
                    // or inside double quotes for string interpolation
                    echo "username is $USERNAME"

                    sh 'echo $WORKSPACE'
                    sh 'echo $PATH'
                    sh 'aws --version'
                    sh 'aws configure set aws_access_key_id $USERNAME'
                    sh 'aws configure set aws_secret_access_key $PASSWORD'
                    sh 'aws configure set region us-east-2'
                    sh 'aws configure set output json'  

                    /*
                    sh 'echo Deploying the network using Cloudformation...'
                    sh 'aws cloudformation create-stack \
                            --stack-name production \
                            --template-body file://src/ecs/network-with-vpc.yml \
                            --capabilities CAPABILITY_IAM' 
                    sh 'echo Deploying the network using Cloudformation complete.'
                    */
                   
                    sh 'echo Deploying the ecs service using Cloudformation...'                    
                    sh 'aws cloudformation create-stack \
                            --stack-name production \
                            --template-body file://src/ecs/service.yml' 
                    sh 'echo Deploying the ecs service using Cloudformation complete.'
                   
                }
            }
            
        }
    }
}