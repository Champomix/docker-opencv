"# docker-opencv" 

sudo docker build --tag docker-java-py3-opencv420-mvn .
sudo docker image ls
sudo docker tag d2972ba8d09a fjuillet/docker-java-py3-opencv420-mvn:latest
sudo  docker push fjuillet/docker-java-py3-opencv420-mvn:latest