pipeline {
    agent any
    tools{
        maven 'maven-3.10.0'
    }
    stages {
        stage('git') {
            steps {
                git 'https://github.com/MohammedSuhail1002/ashok-maven-web-app.git'
            }
        }
        stage('build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('docker images') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', passwordVariable: 'DOCKER_PWD', usernameVariable: 'DOCKER_USER')]) {
                    sh '''
                        echo "$DOCKER_PWD" | docker login -u "$DOCKER_USER" --password-stdin
                        docker build -t techguftugu001/maven-web-app:v1 .
                        docker push techguftugu001/maven-web-app:v1
                        
                    '''
}
            }
        }
        stage('k8s deployment') {
            steps {
                sh 'kubectl apply -f k8s-deploy.yml'
            }
        }
    }
}
