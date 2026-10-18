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
        stage('SonarQube analysis') {
    steps {
        withSonarQubeEnv('sonar-token') {
        sh "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:5.8.0.7211:sonar "
   
        }
        }
}

    stage ('Nexus Upload'){
        steps {
         nexusArtifactUploader artifacts: [[artifactId: '01-maven-web-app', classifier: '', file: 'target/maven-web-app.war', type: 'war']], credentialsId: 'nexus-server', groupId: 'in.ashokit', nexusUrl: '15.252.241.97:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'ashokit-release-repo', version: '3.0-RELEASE'   
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
