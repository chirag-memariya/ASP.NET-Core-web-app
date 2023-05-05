
//         stage('Push to Docker Registry') {
//             steps {
//                 withCredentials([usernamePassword(credentialsId: 'docker-token', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
//                     sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
//                     sh 'docker tag my-app my-docker-registry/my-app'
//                     sh 'docker push my-docker-registry/my-app'
//                 }
//             }
//         }


pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/dotnet/sdk:7.0'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    environment {
        HOME = '/tmp'
    } 

    stages {
        stage('Build') {
            steps {
                sh 'dotnet restore'
                sh 'dotnet build'
            }
        }

        stage('Test') {
            steps {
                sh 'dotnet test'
            }
        }
        stage('Dockerize') {
            steps {
                sh 'docker build -t my-app .'
            }
        }
    }
}
