### 1. **Write a Dockerfile**:
It is a text document that contains the commands a user would execute on the command line to assemble an image. In this file, you can specify the necessary environments and dependencies. For example, see a Dockerfile below:
```bash 
# Pull the "tomcat" image. The community maintains this image. 
FROM tomcat 

# Copy all files present in the current folder to the "/usr/local/tomcat/webapps" folder 
COPY ./*.* /usr/local/tomcat/webapps 
```
In the example above, every time you create a container, it will have the tomcat web server installed. In addition, all the contents of the current directory will also be copied to the */usr/local/tomcat/webapps* folder of each container. See another Dockerfile [example here](https://github.com/docker/labs/blob/master/beginner/static-site/Dockerfile).   

For more info, see the [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for possible commands you can use. 

### 2. **Build an Image:**
    Use the docker build command to build an image from the Dockerfile. Usually, we execute this command from the same directory where the Dockerfile is present.

```
# This command will look for a Dockerfile in the `pwd`, and create myImage
docker build  --tag myImage  [OPTIONS] path_where_to_store_the_image 
```
You can store your images online at DockerHub as well, so that you/anyone can "pull" them on any other machine, anytime. We can even use the pre-created Docker images maintained by the community.

> You can by-pass the two steps mentioned above by directly pulling a pre-created image from the DockerHub to your local machine, and then create and run containers using that image. For example, you can pull an image by running the command below in your terminal:
```
docker pull tomcat:latest
```
### 3. **Create and run a Container:**
After creating an image, you can use it to create as many Containers as you want on any platform. Each container will have the same environment and dependencies to run a copy of your application. The following command creates and runs a new container:
```
docker run --name myContainer myImage
```