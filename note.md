# Make variables in .env file available in bash script

if [ -f .env ]; then
  export $(echo $(cat .env | sed 's/#.*//g'| xargs) | envsubst)
fi

# build image 

docker build -f ./apache/Dockerfile -t $DOCKER_APACHE_IMAGE --build-arg DOCKER_APACHE_VERSION="$DOCKER_APACHE_VERSION" ./apache/
#--quiet

# starte default httpd image 
docker run -d --rm  -p 80:80 -p 443:443 --name my-apache my-apache:latest

# Stopp and remove all container
docker stop $(docker ps -q -a); docker rm $(docker ps -q -a)
# Remove all docker container
docker rm $(docker ps -aq)

# Remove all unused images
docker rmi $(docker images --filter "dangling=true" -q --no-trunc)


docker-compose -f docker-compose.yml -f docker-compose.prod.yml up --build

# networking

host.docker.internal