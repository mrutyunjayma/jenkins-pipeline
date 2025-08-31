pipeline{
    agent any
    
    environment{
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        IMAGE_NAME = "mjdocker3112/myapp"
        
    }

    stages{
        stage('Checkout'){
            steps{
            https://github.com/mrutyunjayma/jenkins-pipeline.git
            }
        }

        stage('Build Docker Image'){
            steps{
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
            }
        }

        stage('Login to DockerHub'){
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALs_USR --password-stdin'
            }
        }

        stage('Push Docker Image'){
            steps{
                sh 'docker push $IMAGE_NAME:$BUILD_NUMBER'
            }
        }

        stage('Deploy to Kubernetes'){
            steps{
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
    }
}