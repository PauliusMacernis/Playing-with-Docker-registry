A simple Dockerfile.  
We make a simple PHP-FPM image and use Caddy as the web server.    

The Docker Cloud repository we are about to use:  
https://hub.docker.com/r/pauliusmacernis/demo-php-caddy/  

Before we automate building images, let's build and tag the image locally and then push  
it to the Docker repository:  
`docker build -t pauliusmacernis/demo-php-caddy:1.0.0 .`  
`docker tag pauliusmacernis/demo-php-caddy:1.0.0 pauliusmacernis/demo-php-caddy:latest`  

After we build the 1.0.0 image, we tag latest to that image as well. We should see the tagged image if we run docker images after the build completes.  

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

++++++++  

- Select GitHub source provider on cloud.docker.com
- Connect GitHub to allow Docker Cloud to access the GitHub repositories.  
- Select GitHub repository to use as a source repository for Docker Cloud repository.
- Configure build rules. We will have two: master for the `latest` tag and a git `tag/release` for versioning.
  - Master:
    - Source type: `Branch`, 
    - Source: `master`, 
    - Docker Tag: `latest`, 
    - Dockerfile location: `Dockerfile`, 
    - Build context: `/`, 
    - Autobuild: `On`, 
    - Build caching: `On` 
  - Tag/Release:
    - Source type: `Tag`, 
    - Source: `/^[0-9.]+$/`, 
    - Docker Tag: `{sourceref}`, 
    - Dockerfile location: `Dockerfile`, 
    - Build context: `/`, 
    - Autobuild: `On`, 
    - Build caching: `On` 
- Once the tag build rule is added, we can click "Save and Build" to start making master. Make sure that the code is pushed to GitHub before using "Save and Build".

Lets test if it works:  
Push some changes to GitHub repository on master and the new build should start on Docker Cloud.  
Push a tag to the same GitHub repository and the new build of tag should start.  

++++++++  

Add GitLab as the second source:  
`git remote add gitlab git@gitlab.com:sugalvojau/demo-php-caddy.git`  
`git push gitlab master && git push gitlab --tags`  

Sign in to GitLab (https://gitlab.com), should see a "Registry" tab in the project.  

Log in to GitLab's registry and push the latest image in master:  
`docker login registry.gitlab.com -u sugalvojau -p my_password_or_access_token`  

Build the image and tag it for GitLab's Registry:  
`docker build -t registry.gitlab.com/sugalvojau/demo-php-caddy .`  

It's also useful to tag an image for a registry from an existing image:  
```
docker tag \
pauliusmacernis/demo-php-caddy:1.0.0 \ 
registry.gitlab.com/sugalvojau/demo-php-caddy:1.0.0
```

You can also pull down the 1.1.0 image and tag it for GitLab:  
`docker pull pauliusmacernis/demo-php-caddy:1.1.0`  
```
docker tag \
pauliusmacernis/demo-php-caddy:1.1.0 \
registry.gitlab.com/sugalvojau/demo-php-caddy:1.1.0
```  

Push all to Gitlab:  
`docker push registry.gitlab.com/sugalvojau/demo-php-caddy`  

When we push the image changes to GitLab  
`git push gitlab master && git push gitlab --tags`  
, an automatic build is triggered,  
and we can see the build jobs in the "pipelines" section of our project.  


+++++++  

Building the image from GitLab-hosted base image. Just run the following commands in `extended-image` directory:  
`docker build -t ch09-sample-project .`  
`docker run --rm -it -p 8080:2015 ch09-sample-project`  

Open http://localhost:8080  

  

