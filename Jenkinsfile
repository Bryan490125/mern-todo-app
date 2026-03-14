pipeline {
agent any

environment {
    DOCKER_USERNAME = 'bryan490125'
}

stages {

    stage('Build') {
        steps {
            echo 'Build stage'
        }
    }

    stage('Containerise') {
        steps {
            sh 'docker build -t $DOCKER_USERNAME/finead-todo-app:latest .'
        }
    }

    stage('Push') {
        steps {
            withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                sh 'echo $PASS | docker login -u $USER --password-stdin'
                sh 'docker push $DOCKER_USERNAME/finead-todo-app:latest'
            }
        }
    }

    stage('Deploy or Test') {
        steps {
            withCredentials([string(credentialsId: 'mongodb-uri', variable: 'MONGO_URI')]) {
                sh '''
                docker stop todo-app || true
                docker rm todo-app || true
                docker run -d --name todo-app -p 8080:5000 -e MONGODB_URI="${MONGO_URI}" $DOCKER_USERNAME/finead-todo-app:latest
                '''
            }
        }
    }

}
}
