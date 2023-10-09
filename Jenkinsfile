pipeline {
    agent any
    
    stages{
        stage("Code"){
           steps{
              echo "Clonig the code" 
              git url:"https://github.com/shaanmahfooz/django-notes-app.git",branch: "main"
           } 
        }
         stage("Build"){
            steps{
              echo "Building the Image" 
              sh "docker build -t notes-app ."
           }
        }
         stage("Push image to Docker Hub"){
            steps{
              echo "Pushing the image to docker hub" 
              withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
              sh "docker tag notes-app ${env.dockerHubUser}/notes-app:latest"   
              sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
              sh "docker push ${env.dockerHubUser}/notes-app:latest"
              }
           }
        }
         stage("Deploy"){
            steps{
               echo "Deploy the container"
               sh "docker-compose down && docker-compose up -d"
           }
        }
    }
}
