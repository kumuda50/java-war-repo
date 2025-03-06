 pipeline {
    agent { label 'jagent' }

    environment {
        AWS_REGION = 'eu-north-1' // Change as per requirement
        ECR_REPO = '908027389474.dkr.ecr.eu-north-1.amazonaws.com/java-repo'
        IMAGE_NAME = 'java-repo'
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
                sh 'sudo docker build -t java-repo .'
            }
        }

        stage('Push Image to AWS ECR') {
            steps {
                script {
                    sh '''
                    aws ecr get-login-password --region eu-north-1 | sudo docker login --username AWS --password-stdin 908027389474.dkr.ecr.eu-north-1.amazonaws.com
                    sudo docker tag java-repo:latest 908027389474.dkr.ecr.eu-north-1.amazonaws.com/java-repo:latest
                    sudo docker push 908027389474.dkr.ecr.eu-north-1.amazonaws.com/java-repo:latest
                    '''
                }
            }
        }
    }
}
