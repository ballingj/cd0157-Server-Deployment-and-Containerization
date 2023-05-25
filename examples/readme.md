## Simple Container from a Dockerfile 
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

## Creating Container from Dockerhub Exercise Instructions
Prerequisite:  Download and install the PostgreSQL before you continue the exercise below.

Exercise Steps
### 1. Fetch the image

Find the Postgres image by searching on DockerHub, or use this link to go directly to the image page. Postgres is an open source relational database, and the Postgres Docker image has the database software installed.

Run the following command in your terminal to download the latest Postgres image to your machine:

```
docker pull postgres:latest
```

### 2. Create and run a container

Run the image using the docker run command, supplying a password for Postgres:
```
docker run --name psql -e POSTGRES_PASSWORD=password! -p 5433:5432 -d postgres:latest
```
> Note: If you have Postgres already installed and running locally, then use a different local port other than 5432, such as 5433.

In the command above:

- The ```--name``` flag allows you to specify a name for the container that can be used later to reference the container. If you don’t specify a name, Docker will assign a random string name to the container.
- The -e flag stands for “environment”. This sets the environment variable POSTGRES_PASSWORD to the value password!.
- The ```-p``` flag stands for “publish”. This allows you to bind your local machine’s port 5433 to the container port 5432.
- The ```-d``` stands for “detach”. This tells Docker run the indicated image in the background and print the container ID. When you use this command, you will still be able to use the terminal to run other commands, otherwise, you would need to open a new terminal.

### 3.Connect to the container

If you have the Postgres client installed (prerequisite), you can use it to connect with the Docker container database using the following command:
```
psql -h 127.0.0.1 -p 5433 -U postgres
# Mac/Linux users can also connect from the 0.0.0.0 address
```
> Facing issues? See the Troubleshooting steps below.

This command allows you to access the database using the same port that you exposed earlier. Note that after running that command you will need to enter the same password that you set with the ```POSTGRES_PASSWORD``` when creating the container (```password!```).

### 4. Test the container with some SQL commands. For example,
- List all databases using ```\l``` command or
- List all tables (relations) using the ```\dti``` command.
- More commands can be found in the Postgres documentation here https://www.postgresql.org/docs/current/app-psql.html.
- When you are finished testing Postgres, you can quit your connection to Postgres using ```\q```

### 5. Clean-up

To stop the running Docker container, you will first need to find it. You can list the running containers with the command docker ps. Copy the id of your Postgres container. Then, You can stop a particular container using its ID.
```
#List all containers
docker ps --all
# Stop
docker stop <container_ID>
# Remove
docker container rm <container_ID>
```
Similarly, you can view the images in your local system, and remove anyone as:
```
# List all images
docker image ls
# Remove
docker image rm <image_
```

### Troubleshoot

#### 1. All users
- The default IP of the (local) host machine is 127.0.0.1. Therefore, use 127.0.0.1 instead of 0.0.0.0 while connecting with the container Postgres database:
```
psql -h 127.0.0.1 -p 5433 -U postgres
```
See this discussion about the difference between localhost and 0.0.0.0

#### 2. Windows users
- We recommend you to use WSL. However, if you are still using CMD or GitBash, then run the CMD/GitBash as an Administrator.

- If you have a version other than Windows 10 Home, you will have to find the host IP address, using the ```ipconfig``` command in your CMD. Look out for the Ethernet adapter vEthernet IPv4 address, such as ```192.168.__.___```. Use that IP address in the command while connecting with the container Postgres:
```
psql -h 192.168.__.___ -p 5433 -U postgres
```
