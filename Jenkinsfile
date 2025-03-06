 pipeline {
    agent { label 'agent-build' }

    environment {
        AWS_REGION = 'eu-north-1' // Change as per requirement
        ECR_REPO = '908027389474.dkr.ecr.eu-north-1.amazonaws.com/java-repo'
        IMAGE_NAME = 'java-repo'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/kumuda50/java-war-repo.git'
            }
        }

        stage('Build Java Application') {
            steps {
                sh 'mvn clean package -DskipTests' // Ensure Maven is installed on the agent
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'sudo docker build -t ecr-repo .'
            }
        }

        stage('Push Image to AWS ECR') {
            steps {
                script {
                    sh '''
                    aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin 116981760941.dkr.ecr.us-east-1.amazonaws.com
                    sudo docker tag ecr-repo:latest 116981760941.dkr.ecr.us-east-1.amazonaws.com/ecr-repo:latest
                    sudo docker push 116981760941.dkr.ecr.us-east-1.amazonaws.com/ecr-repo:latest
                    '''
                }
            }
        }
    }
}
