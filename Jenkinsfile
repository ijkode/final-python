pipeline {
    agent {
        label 'docker'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/master']],
                          userRemoteConfigs: [[url: 'https://github.com/ijkode/final-python']]])
            }
        }

        stage('Build') {
            steps {
                sh "docker build -t final-python:${env.BUILD_NUMBER} ."
            }
        }

        stage('Run & Test') {
            steps {
                sh "docker run --name final-python -d -p 8081:5000 final-python:${env.BUILD_NUMBER}"
                sh "sleep 5"
                sh "curl http://3.80.24.157:8081/api/doc"
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials(bindings:[usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    sh "docker tag final-python:${env.BUILD_NUMBER} lirlib/final-python:${env.BUILD_NUMBER}"
                    sh "docker login -u $user -p $pass "
                    sh "docker push lirlib/final-python:${env.BUILD_NUMBER}"
                }
            }
 }
        
    }
}