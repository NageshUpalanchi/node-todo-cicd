pipeline {
    agent any
    stages {
        stage("Clone the cone") {
            steps {
                echo "Cloing the code"
                git url:"https://github.com/NageshUpalanchi/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Build the code") {
            steps {
                echo "Building the code"
                sh "docker build . -t node-cicd"
            }
        }
        stage("Push to DockerHub") {
            steps {
                
                echo "Creating variable, and tagging the image name"                
                withCredentials([usernamePassword(credentialsId:"DockerHub", passwordVariable:"DockerPass",usernameVariable:"DockerUser")]){
                    
                sh "docker tag node-cicd ${env.DockerUser}/node-cicd:latest"
                    
                echo "Login to DockerHub"
                sh "docker login -u ${env.DockerUser} -p ${env.DockerPass}"

                echo "Pushing image to DockerHub"
                sh "docker push ${env.DockerUser}/node-cicd:latest"
                    
                echo "DockerHub push is successfully done"
                }
             
            }
            
        }
        stage("Deploy the code") {
            steps {
                echo "Stop and removing the containers"
                
                sh "docker stop \$(docker ps -a -q)" 
                sh "docker rm \$(docker ps -a -q)"

                echo "Deploy the code"
                sh "docker-compose up -d"
                
            }
        }
    }
}
