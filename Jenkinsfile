pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
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

        stage('Push to Docker Registry') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-token', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    sh 'docker tag my-app my-docker-registry/my-app'
                    sh 'docker push my-docker-registry/my-app'
                }
            }
        }

//         stage('Deploy') {
//             steps {
//                 sh 'kubectl apply -f deployment.yml'
//             }
//         }
    }
}




// pipeline {
//     agent {
//         docker {
//             image 'mcr.microsoft.com/dotnet/sdk:7.0'
//             args '-v /var/run/docker.sock:/var/run/docker.sock'
//         }
//     }
//     environment {
//         HOME = '/tmp'
//     } 

//     stages {
//         stage('Build') {
//             steps {
//                 sh 'dotnet restore'
//                 sh 'dotnet build'
//             }
//         }

//         stage('Test') {
//             steps {
//                 sh 'dotnet test'
//             }
//         }

//         stage('Publish') {
//             steps {
//                 sh 'dotnet publish -c Release -o out'
//             }
//         }

//         stage('Dockerize') {
//             steps {
//                 script {

//                     withDockerRegistry(credentialsId: 'dockerhub-token', toolName: 'docker', url: 'https://registry.hub.docker.com/') {
//                         // some block
//                         def image = docker.build('memariyachirag126/mywebapp:1.0', '.')
//                         image.push()
//                     }

//                     docker.withRegistry('https://registry.hub.docker.com', 'docker-token') {
//                         def image = docker.build('memariyachirag126/mywebapp:1.0', '.')
//                         image.push()
//                     }
//                 }
//             }
//         }

//         stage('Deploy') {
//             steps {
//                 script {
//                     withDockerRegistry(credentialsId: 'dockerhub-token', toolName: 'docker', url: 'https://registry.hub.docker.com/') {
//                         def image = docker.image('memariyachirag126/mywebapp:1.0')
//                         docker.image('memariyachirag126/mywebapp:1.0').run('--rm -it -p 8000:80')
//                     }
//                 }
//             }
//         }
//     }
// }
