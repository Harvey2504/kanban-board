pipeline{
    agent any

    stages{

    
        /*stage("docker build"){
            steps{
                sh "docker-compose up -d"
            }
        }

        stage("commiting the docker images"){
            steps{
                sh "docker commit kanban-ui harvey2504/kanban-ui:v2"
                sh "docker commit kanban-app harvey2504/kanban-app:v2" 
            }
        }

        stage("pushing the images to docker hub"){
            steps{
                withDockerRegistry([ credentialsId: "7c552b6c-f6d2-4dec-9af4-29d8a2199cdf", url: "" ]){
                    sh "docker push harvey2504/kanban-app:v2"
                    sh "docker push harvey2504/kanban-ui:v2"
                }
            }
        }*/

        stage("kubernetes"){
            steps{
                sh "kubectl apply -f kubernetes"
                //sh "minikube service ui-service"
            }
        }
    }
}