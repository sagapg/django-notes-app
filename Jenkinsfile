pipeline {
    agent any
    
    stages{
        stage("code"){
            steps {
                echo "cloning the code"
                git url:"https://github.com/sagapg/django-notes-app.git", branch: "main"
            }
        }
        stage("build"){
            steps {
                echo "build the image"
                sh " sudo docker build -t my-notes-app ."
            }
        }
        stage("push to DockerHub"){
            steps {
                echo "pushing image to dockerHub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag my-notes-app ${env.dockerHubUser}/my-notes-app:latest "
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-notes-app:latest"
                }
            }
        }
        stage("depoy"){
            steps {
              echo "deploying a container"
              sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
