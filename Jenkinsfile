pipeline {
    agent { label 'jagent' }

    environment {
        AWS_REGION = 'eu-north-1' // Change as per requirement
        ECR_REPO = '908027389474.dkr.ecr.eu-north-1.amazonaws.com/java-repo'
        IMAGE_NAME = 'santhosh-application'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Santhoshsb1/java-war-repo.git'
            }
        }

        stage('Build Java Application') {
            steps {
                sh 'mvn clean package -DskipTests' // Ensure Maven is installed on the agent
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'sudo docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push Image to AWS ECR') {
            steps {
                script {
                    sh '''
                    aws ecr get-login-password --region $AWS_REGION | sudo docker login --username AWS --password-stdin $ECR_REPO
                    sudo docker tag $IMAGE_NAME:latest $ECR_REPO/$IMAGE_NAME:latest
                    sudo docker push $ECR_REPO/$IMAGE_NAME:latest
                    '''
                }
            }
        }
    }
}
