pipeline {
    agent any 
    
     environment {
                WORKSPACE=pwd()
                ETAG=''
    }
    stages {
        stage('Build'){
            withCredentials([usernamePassword(credentialsId: 'd36307c6-83c6-4f78-b415-a8af2b648f54', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                // available as an env variable, but will be masked if you try to print it out any which way
                // note: single quotes prevent Groovy interpolation; expansion is by Bourne Shell, which is what you want
                sh 'echo $PASSWORD'
                // also available as a Groovy variable
                echo USERNAME
                // or inside double quotes for string interpolation
                echo "username is $USERNAME"
            }
            steps{
                sh 'echo building...'
                git url: 'https://github.com/dancurrotto/cloud-architecture-infrastucture.git'
            }
        }
        stage('Deploy') {
            steps {
                
                sh 'echo $WORKSPACE'
                sh 'echo $AWS_ACCESS_KEY_ID'
                sh 'echo $PATH'
                sh '~/.local/bin/aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID'
                sh '~/.local/bin/aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY'
                sh '~/.local/bin/aws configure set region us-east-2'
                sh '~/.local/bin/aws configure set output json'  

                    
                
               
                
            }
            
        }
    }
}