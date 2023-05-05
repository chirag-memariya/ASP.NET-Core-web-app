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

        stage('Publish') {
            steps {
                sh 'dotnet publish -c Release -o out'
            }
        }

        stage('Dockerize') {
            steps {
                script {

                    docker.withRegistry('https://registry.hub.docker.com', 'docker-token') {
                        def image = docker.build('memariyachirag126/mywebapp:1.0', '.')
                        image.push()
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-token') {
                        def image = docker.image('memariyachirag126/mywebapp:1.0')
                        docker.image('memariyachirag126/mywebapp:1.0').run('--rm -it -p 8000:80')
                    }
                }
            }
        }
    }
}
