pipeline {
    agent any

    stages {
        
        
        stage('Code') {
            steps {
                git url: "https://github.com/faizan35/todo-app-python", branch: "main"
                echo 'Code has beed cloned...'
            }
        }

        stage('Build') {
            steps {
                sh "docker build -t todo-app ."
                echo 'Building Docker image Completed.'
            }
        }

        stage('Scan') {
            steps {
                echo 'Scanning Done.'
            }
        }

        stage('Push') {
            steps {
                
                withCredentials([usernamePassword(credentialsId:"dockerHub", passwordVariable:"dockerHubPass", usernameVariable:"dockerHubUser")]){
                   sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                   sh "docker tag todo-app ${env.dockerHubUser}/todo-app:latest"
                   sh "docker push ${env.dockerHubUser}/todo-app:latest"
                   echo 'Pushed to DockerHub.' 
                }
                
            }
        }

        stage('Deploy') {
            steps {
                sh "docker-compose down && docker-compose up -d"
                echo 'Deployed Done.'
            }
        }

    }
}
