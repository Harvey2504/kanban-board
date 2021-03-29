# Jenkins Pipeline Integration .

## Pre-Requisites
* Install Jenkins through this way (for jenkins image)
1.Go to your cmd run this command
```

docker run -p 8080:8080 -v /var/run/docker.sock:/var/run/docker.sock --name jenkins jenkins/jenkins:lts

```
we will get an initial admin password do the jenkins installation steps 

After completion of jenkins installation

2.To enter into your docker image run this command
```
docker exec -it -u root jenkins bash

```

3.install docker in docker
```
apt-get update && \
apt-get -y install apt-transport-https \
     ca-certificates \
     curl \
     gnupg2 \
     software-properties-common && \
curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg > /tmp/dkey; apt-key add /tmp/dkey && \
add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
   $(lsb_release -cs) \
   stable" && \
apt-get update && \
apt-get -y install docker-ce
```


now you will be inside your docker and run '''docker ps''' you will get something like this
```
root@cf1fd5908a1c:/# docker ps
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                               NAMES
cf1fd5908a1c        jenkins/jenkins:lts   "/sbin/tini -- /usr/…"   2 hours ago         Up 12 minutes       0.0.0.0:8080->8080/tcp, 50000/tcp   jenkins
root@cf1fd5908a1c:/#   
```
this means you have succesfully created the image 

Now to give jenkins actions access to all users for that run this command
```
chmod 777 /var/run/docker.sock
```

* Download docker compose
link : https://docs.docker.com/compose/install/
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.28.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
To install a different version of Compose, substitute 1.28.5 with the version of Compose you want to use.
* Apply executable permissions to the binary
```
chmod +x /usr/local/bin/docker-compose
```




## Setting Up Jenkins
### Configure Credentials
```
Manage Jenkins > Manage Credentials 
Add Credentials
kind : Username with Password
Username : harvey2504 (dockerhub)
Password : Protag***** (dockerhub)
Save and apply and use the ID generated in the pipeline .
```

### Configure Declarative Pipeline (Docker)
```
Manage Jenkins > Config System > Declarative Pipeline (Docker)
Docker Label : docker
Docker registry URL :
Registry credentials : choose correct credentials from dropdown menu
Apply and Save.
```


## Pipeline Stages Script
### Build Stage
```
stage("docker build"){
            steps{
                sh "docker-compose up -d"
            }
        }
```
### Commit Stage (TAG)
```
stage("commiting the docker images"){
            steps{
                sh "docker commit kanban-ui harvey2504/kanban-ui:v2"
                sh "docker commit kanban-app harvey2504/kanban-app:v2" 
            }
        }
```
### Push Stage 
```
stage("pushing the images to docker hub"){
            steps{
                withDockerRegistry([ credentialsId: "7c552b6c-f6d2-4dec-9af4-29d8a2199cdf", url: "" ]){
                    sh "docker push harvey2504/kanban-app:v2"
                    sh "docker push harvey2504/kanban-ui:v2"
                }
            }
        }
```
