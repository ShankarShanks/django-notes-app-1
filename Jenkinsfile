pipeline{
    agent any
    
    stages{
        stage("SCM Code"){
            steps{
                echo "Clone the code"
                git branch: 'main', url: 'https://github.com/ShankarShanks/django-notes-app-1.git'
            }
        }
        stage("Build"){
            steps{
                echo "Building the code"
                sh "docker build -t notes-app ."
            }
        }
        stage("Push to DockerHub"){
            steps{
                echo "Pushing the image to DockerHub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag notes-app ${env.dockerHubUser}/notes-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/notes-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
    
}
