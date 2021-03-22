pipeline{
    agent {
        docker { image 'node:14-alpine' }
    }

    stages{

    
        stage("docker build"){
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
                withDockerRegistry([ credentialsId: "07790bf0-f91c-4f25-bf4c-afefc4008cc7", url: "" ]){
                    sh "docker push harvey2504/kanban-app:v2"
                    sh "docker push harvey2504/kanban-ui:v2"
                }
            }
        }

        stage("kubernates"){
            steps{
                sh "kubectl apply -f kubernetes"
                //sh "minikube service ui-service"
            }
        }
    }
}