pipeline{
    agent any
    
    stages{
        stage("Code"){
            steps{
                echo "clonning the code" 
                git url:"https://github.com/vishalbambare/RiskAssess.git" , branch: "master"          
                
            }
            
        }
        
        stage("Build"){
            steps{
                echo "Building the code" 
                sh "docker build -t riskassess ."
            }
            
        }
        stage("Push To Docker Hub"){
            steps{
                echo "Push Image to the Docker Hub" 
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag riskassess ${env.dockerHubUser}/riskassess:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/riskassess:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying the code" 
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
