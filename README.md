A simple Dockerfile.  
We make a simple PHP-FPM image and use Caddy as the web server.    

The Docker Cloud repository we are about to use:  
https://hub.docker.com/r/pauliusmacernis/demo-php-caddy/

Before we automate building images, let's build and tag the image locally and then push
it to the Docker repository:  
`docker build -t pauliusmacernis/demo-php-caddy:1.0.0 .`  
`docker tag pauliusmacernis/demo-php-caddy:1.0.0 pauliusmacernis/demo-php-caddy:latest`  

After we build the 1.0.0 image, we tag latest to that image as well. We should see the
tagged image if we run docker images after the build completes.  

To push the image to Docker Hub, we need to verify our credentials on the commandline with the `docker login` command. 
By default running the `login` command logs us into the Docker Hub registry (https://hub.docker.com).  
Read more:  
https://docs.docker.com/engine/reference/commandline/login/  

Log in to a Docker registry:  
`docker login -u pauliusmacernis`  

Push the image:  
`docker push pauliusmacernis/demo-php-caddy`  

Lets test if it works:  
`docker rmi pauliusmacernis/demo-php-caddy:1.0.0`  
`docker images` (the image `demo-php-caddy:1.0.0` should not be in the list)    
`docker pull pauliusmacernis/demo-php-caddy:1.0.0`  
`docker images` (the image `demo-php-caddy:1.0.0` should be in the list)  







