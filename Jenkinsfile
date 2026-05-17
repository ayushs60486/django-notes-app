@Library('shared') _
pipeline{
    
    agent{
        label 'vinod'
    }
    
    stages{
        
        stage("hello"){
            steps{
                script{
                    hello()
                }
            }
        }
        
        stage("Code"){
            steps{
               script{
                   clone("https://github.com/ayushs60486/django-notes-app.git","main")
               }
            }
        }
        stage("Build"){
            steps{
                echo "this is building the code"
                sh "docker build -t notes-app:latest ."
            }
        }
        stage("Pushing to DH"){
            steps{
                echo "this is pushing the code to DH"
                withCredentials([usernamePassword(
                    'credentialsId':"DockerhubCred",
                    passwordVariable:"dockerHubPass",
                    usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag notes-app:latest ${env.dockerHubUser}/notes-app:latest"
                sh "docker push ${env.dockerHubUser}/notes-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "this is deploying the code"
                sh "docker compose down" 
                sh "docker compose up -d"
                echo "the code is deployed"
            }
        }
    }
}
