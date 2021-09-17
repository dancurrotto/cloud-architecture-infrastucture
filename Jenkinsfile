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
                    sh 'echo $WORKSPACE'
                    sh 'echo $PATH'
                    sh 'aws --version'
                    sh 'aws configure set aws_access_key_id $USERNAME'
                    sh 'aws configure set aws_secret_access_key $PASSWORD'
                    sh 'aws configure set region us-east-2'
                    sh 'aws configure set output json' 
                    

                    sh '''
                        if ! aws cloudformation describe-stacks --stack-name production ; then 
                            echo -e "Stack does not exist, creating network(production) stack..." 

                            aws cloudformation create-stack \
                                --stack-name production \
                                --template-body file://src/ecs/network-with-vpc.yml \
                                --capabilities CAPABILITY_IAM

                            aws cloudformation wait stack-create-complete \
                                --stack-name production

                        else \
                            echo -e "Stack exists, attempting updating network(production) stack ..."  

                            aws cloudformation update-stack \
                                --stack-name production \
                                --template-body file://src/ecs/network-with-vpc.yml \
                                --capabilities CAPABILITY_IAM   

                            aws cloudformation wait stack-update-complete \
                                --stack-name production             
                        fi
                        '''

                     sh '''
                        if ! aws cloudformation describe-stacks --stack-name ecs-service ; then 
                            echo -e "Stack does not exist, creating ecs-service stack..." 

                            aws cloudformation create-stack \
                                --stack-name ecs-service \
                                --template-body file://src/ecs/network-with-vpc.yml \
                                --capabilities CAPABILITY_IAM 

                            aws cloudformation wait stack-create-complete \
                                --stack-name ecs-service

                                
                        else \
                            echo -e "Stack exists, attempting updating ecs-service stack ..." 

                            aws cloudformation update-stack \
                                --stack-name ecs-service \
                                --template-body file://src/ecs/network-with-vpc.yml \
                                --capabilities CAPABILITY_IAM   

                            aws cloudformation wait stack-update-complete \
                                --stack-name ecs-service               
                        fi
                        '''
                }
            }
            
        }
    }
}