pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS= credentials('dockerhub_id')
    }
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/jfdelafuente/two-tier-flask-app.git", branch: "master"
                echo 'Git Checkout Completed'
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker build . -t flaskapp"
                echo 'Build Image Completed'
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag flaskapp ${env.dockerHubUser}/flaskapp:latest"
                    sh "docker push ${env.dockerHubUser}/flaskapp:latest" 
                }
                echo 'Login Completed'
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
