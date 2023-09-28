pipeline {
    agent any
    stages {
        stage("Clone Code") {
            steps {
               echo "Cloning the Code"
               git url:"https://github.com/ShubhamMahile/django-notes-app.git", branch:"main"
            }
        }
        stage("Build") {
            steps {
               echo "Building an Image"
               bat "docker build -t my-note-app ."
            }
        }
         stage("Push to docker hub") {
            steps {
                 echo "Pushing an Image to Docker Hub"
                 withCredentials([usernamePassword(credentialsId: "dockerHub", usernameVariable: "dockerHubUser", passwordVariable: "dockerHubPass")])
                 {
                   bat "docker tag my-note-app ${env.dockerHubUser}/my-note-app:latest"
                   bat "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                   bat "docker push ${env.dockerHubUser}/my-note-app:latest"
                 }
            }
        }
        stage("Deploy") {
            steps {
                 echo "Deploying Container"
                 //bat "docker run -d -p 8000:8000 shubhammahile/my-note-app:latest"
                 bat "docker-compose down && docker-compose up -d"
            }
        }
    }
}
