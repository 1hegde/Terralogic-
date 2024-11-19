Build the docker image using

docker build -t my-node-app .

Push it to the container using 

docker login
docker tag my-node-app your-username/my-node-app:latest     [ username= dockerhub usernamer ]
docker push your-username/my-node-app:latest
