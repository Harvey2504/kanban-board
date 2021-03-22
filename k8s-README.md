Docker Login
```
docker login
```
Build Separate images for app and ui
```
docker build -t harvey2504/kanban-app:v1 . 

docker build -t harvey2504/kanban-ui:v1 . 
```
push it to docker hub
```
docker image push harvey2504/kanban-app:v1

docker image push harvey2504/kanban-ui:v1
```