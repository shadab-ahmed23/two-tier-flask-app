pipeline {
    agent any

    stages {
        stage('Code') {
            steps {
                echo 'Cloning the code from GitHub' 
                git url:"https://github.com/shadab-ahmed23/two-tier-flask-app.git",branch:"eks"
            }
        }
        stage('Build') {
            steps {
                echo 'Building the code from '
                sh "docker build -t flask-app ."
            }
        }
        stage('push to dockerHub') {
            steps {
                echo 'push the image to dockerHub'
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker tag my-note-app shadabahmed23/flask-app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                     sh "docker push shadabahmed23/flask-app:latest"
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying the code'
                sh "docker run -d -p 5000:5000 shadabahmed23/flask-app:latest"
            }
        }
    }
}
