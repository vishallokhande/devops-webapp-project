pipeline {
  agent any
  environment {
    AWS_REGION = 'ap-south-1'
    ECR_REGISTRY = '704444257628.dkr.ecr.$AWS_REGION.amazonaws.com'
    ECR_REPO = 'devops-sample-app'
    IMAGE_TAG = "${env.BUILD_NUMBER}"
  }
  stages {
    stage('Checkout') { steps { checkout scm } }
    stage('Build Docker') {
      steps {
        sh 'docker build -t $ECR_REPO:$IMAGE_TAG ./app'
      }
    }
    stage('Authenticate ECR') {
      steps {
        sh '''
          aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin ${ECR_REGISTRY}
          docker tag $ECR_REPO:$IMAGE_TAG ${ECR_REGISTRY}/$ECR_REPO:$IMAGE_TAG
        '''
      }
    }
    stage('Push to ECR') {
      steps {
        sh 'docker push ${ECR_REGISTRY}/$ECR_REPO:$IMAGE_TAG'
      }
    }
    stage('Deploy to EKS') {
      steps {
        sh '''
          # update image in k8s manifest and apply
          sed -i "s|:latest|:$IMAGE_TAG|g" k8s/deployment.yaml || true
          kubectl apply -f k8s/deployment.yaml
        '''
      }
    }
  }
}

