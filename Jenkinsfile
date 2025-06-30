pipeline {

    agent {label "vinod"}
    
    stages {
        stage("Cloning the Code") {
            steps {
                echo "--- Cloning the Code from GitHub : Start ---"
                git url: "https://github.com/ritwik-satpati/django-notes-app", branch: "main"
                echo "--- Cloning the Code from GitHub : Complete ---"
            }
        }
        stage("Build Docker Image") {
            steps {
                echo "--- Building the Code as a Docker Image : Start ---"
                sh "docker build -t notes-app:latest ."
                echo "--- Building the Code as a Docker Image : Complete ---"
            }
        }
        stage("Push to DockerHub") {
            steps {
                echo "--- Pushing the Image to DockerHub : Start ---"
                withCredentials([usernamePassword(credentialsId:"dockerHubCred", usernameVariable:"dockerHubUser", passwordVariable:"dockerHubPass")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag notes-app:latest ${env.dockerHubUser}/notes-app:latest"
                    sh "docker push ${env.dockerHubUser}/notes-app:latest"
                }
                echo "--- Pushing the Image to DockerHub : Complete ---"
            }
        }
        stage("Deploy the Code") {
            steps {
                echo "--- This is Deploying the Code using Docker Image : Start ---"
                // sh "docker run -d -p 8000:8000 notes-app:latest"
                sh "docker compose up -d"           // take inpur form - docker-compose.yml
                echo "--- This is Deploying the Code using Docker Image : Complete ---"
            }
        }
    }
}
