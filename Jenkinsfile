pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-1"
        ECR_REPO = "350480401937.dkr.ecr.ap-south-1.amazonaws.com/app-a"
        IMAGE_TAG = "v1"
    }

    stages {

        stage('Build') {
            steps {
                sh 'chmod +x mvnw'
                sh './mvnw clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $ECR_REPO:$IMAGE_TAG .'
            }
        }

        stage('ECR Login') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION \
                | docker login --username AWS --password-stdin $ECR_REPO
                '''
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $ECR_REPO:$IMAGE_TAG'
            }
        }

        stage('Deploy to Kubernetes') {
    steps {
        sh '''
            pwd
            find . -maxdepth 3 -type f | sort
        '''
    }
}

    //     stage('Deploy to Kubernetes') {
    //         steps {
    //              withCredentials([string(credentialsId: 'k8s-token', variable: 'K8S_TOKEN')]) {
    //             sh '''
    //                 kubectl \
    //                 --server=https://10.0.1.132:6443 \
    //                 --token="$K8S_TOKEN" \
    //                 --insecure-skip-tls-verify=true \
    //                 apply -f k8s/deployment.yaml

    //                 kubectl \
    //                 --server=https://10.0.1.132:6443 \
    //                 --token="$K8S_TOKEN" \
    //                 --insecure-skip-tls-verify=true \
    //                 apply -f k8s/service.yaml
    //             '''
    //     }
    // }
//}
    }
}