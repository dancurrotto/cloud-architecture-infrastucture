pipeline {
    agent any 
    
     environment {
                WORKSPACE=pwd()
                ETAG=''
    }
    stages {
        stage('hello AWS') {
            steps {
                withAWS(credentials: 'd36307c6-83c6-4f78-b415-a8af2b648f54', region: 'us-east-2') {
                    sh 'echo "hello KB">hello.txt'
                    s3Upload acl: 'Private', bucket: 'kb-bucket', file: 'hello.txt'
                    s3Download bucket: 'kb-bucket', file: 'downloadedHello.txt', path: 'hello.txt'
                    sh 'cat downloadedHello.txt'
                }
            }
        }
        /*
        stage('Build'){
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

                sh 'echo Listing s3 buckets...'
                sh '~/.local/bin/aws aws s3 ls'              
                
               
                
            }
            
        }
        */
    }
}