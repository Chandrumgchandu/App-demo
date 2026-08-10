pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/Chandrumgchandu/App-demo.git'
            }
        }

        stage('Build') {
            steps {
                sh './mvnw clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t app-a:v1 .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8081:8080 app-a:v1'
            }
        }
    }
}