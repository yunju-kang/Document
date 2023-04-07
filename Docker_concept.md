```
sudo systemctl enable docker
sudo systemctl start docker
```

```
sudo usermod -a -G docker user
grep docker /etc/group
```



### Installation

##### Installation Blog

https://sh-tsang.medium.com/installation-of-docker-3b18d9e70bea

##### Order

​	`sudo apt-get update`

1. Remove existing Docker installs

2. Install package to allow Apt

   `sudo apt-get install apt-transport-https ca-certificates curl software-properties-common`

3. Add the GPG key and Docker repository

   (GPG Key)

   `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`

   `sudo apt-key fingerprint 0EBFCD88`

   (repository)

   `sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`

4. Install the docker-ce, docker-ce-cli, and containerd.io packages

   `sudo apt install docker-ce docker-ce-cli containerd.io`

5. add the user to the docker group

   `sudo usermod user_name -aG docker`

##### Launching HelloWorld

`docker run hello-world`



### Container

1. ##### List of container

   (container running)

   `docker container ls`

   (all container)

   `docker container ls -a`

2. ##### Docker run(new container)

   `docker run alpine`

   (familiar structure)

   `docker run -i alpine`

   (more familiar structure)

   `docker run -it --name a-container alpine`

   (back-ground running + restart anytime)

   `docker run -dt --restart always --name bg-container alpine`

   (delete container after running)

   `docker run -it --name rm-test --rm alpine`

3. ##### Docker run(existing container)

   `docker start a-container`

4. ##### Accessing the container

   Run a command against a container

   `docker exec <container> <command>`

   ex)`docker a-container apk add nginx`

   ex) `docker exec a-container cat /etc/nginx/conf.d/default.conf`

   Drop into a shell

   `docker exec -it <container> <shell>`

   ex) `docker exec -it a-container ash`

   Copy file to container / copy container file to localhost

   `docker cp <source> <container>:<destination>`

   `docker cp <container>:<source> <destination>`

5. ##### Container Management

   Stopping container

   `docker stop <container>`

   Restarting container

   `docker restart <container>`

   Removing container

   (specific container)

   `docker rm <container> `

   (all stopped container)

   `docker container prune`

   Renaming container

   `docker rename <container> <new_name>`

   Viewing container metrics

   `docker stats`

6. ##### Container Publishing

   Container information

   `docker inspect <container>`

   (+ find word)

   `docker inspect <container> | grep <word>`

   Container to image

   `docker commit <container> <image_name>`

   Map container port to host server

   `docker run -p <host_port>:<container_port> <container>`





### Image

1. ##### Concecpt

   - Images are comprimised of cached layers 
     -> increases reusability, decrease disk space, and ensure fast deploy speeds


   - Base image : the first layer defining the file system. image based on scratch template
   - Parent image : the image the curent docker image is based on

2. ##### Building a docker file

   provide a parent/scratch image

   `FROM <image|scratch>`

   Run a command 

   `RUN <command>`

   Work directory

   `WORKDIR <directory/path>`

   Copy files

   `COPY --chown <user> <source> <destination>`

   Copy files (available from Url)

   `Add <source> <destination>`

   Switch to user

   `USER <user>`

   Expose a port

   `EXPOSE <port>`

   Single command that run when container starts

   `CMD ["<executable>", "<param1>", "<param2>"]`

3. ##### Image Management

   build an image

   `docker image build -t <name>:<tag> <path>`

   ex) `docker image build -t exercise_image01:v1 .`

   save an image locally

   `docker pull <image>`

   Images list

   `docker image ls`

   Remove images

   `docker image rm <image>`

   Prune all unused images

   `docker image prune -a`

   View image information

   `docker image inspect <image>`

4. ##### Docker hub

   Login docker hub

   `docker login --username=<user_name>`

   Tag image for a repository

   `docker tag <image> <user_name>/<repo>:<version>`

   Push to repo

   `docker push <user_name>/<repo>`

   Logout Docker

   `dockcer logout`



### Networking

1. Networking Commands

   Create network

   `docker network create <network_name>`

   Remove network

   `docker network rm <network_name>`

   Connecting a container to a network

   `docker network connect <network> <container>`

   Removing a container from a network

   `docker network disconnect <network> <container>`

2. Networking Container

   Run container with network

   `docker container run --name <container_name> -it --network <network> <container>`

   ​

### Volume

<source> can be local directory(or file) or volume.

1. ##### Bind mountain

   Using the mount flag

   `docker run -d --mount type=bind,source=<source>,target=<container_target_directory> <image>`

   Using the volume flag

   `docker container run -d -v <source>:<target> <imge>`

   ex) `docker container run -d --name nginx-bind-mount2 -v "$(pwd)"/target2:/appnginx`

2. ##### Using volumes for storage

   Create a new volume

   `docker volume create html-volume`

   Using the mount flag

   `docker container run -d --mount type=volume,source=<source>,target=<target> <image>`

   Creating a volume using the volume flag

   `docker container run -d -v <volume_name>:<target> <image>`




### Dockerfile

1. ##### Environment Variables

```dockerfile
FROM node
LABEL org.label-schema.version=v1.1
ENV NODE_ENV="development"
ENV PORT 3000
RUN mkdir -p /var/node
ADD src/ /var/node/
WORKDIR /var/node
RUN npm install
EXPOSE $PORT
CMD ./bin/www
```

`docker container run -d --name <container> -p <num>:<num> --env PORT=<value> NODE_ENV=<value> <image>`

`ENV` values are available to containers.

2. ##### Build Arguments

```dockerfile
FROM node
LABEL org.label-schema.version=v1.1
ARG SRC_DIR=/var/node
RUN mkdir -p $SRC_DIR
ADD src/ $SRC_DIR
WORKDIR $SRC_DIR
RUN npm install 
EXPOSE 3000
CMD ./bin/www
```

`docker image build -t <name>:<tag> --build-arg SRC_DIR=<value> <path>`

`ARG` is only available during the build of a docker image.

If you don't set `arg` when you use `image build` command, `arg` will use `arg` value in the docker file.

3. ##### Non-privileged Users

```dockerfile
FROM centos:latest
RUN useradd -ms /bin/bash cloud_user
USER cloud_user
```

If you use `-u` flag, then the user will be `-u` flag's value not docker file's user value.

​	Connecting as privileged user
​	: `0` means 'root'.

​	`docker container exec -u 0 -it test-build /bin/bash`

​	Check who is the user

​	`whoami`

4. ##### Order of Execution

```dockerfile
# before
FROM centos:latest
RUN mkdir -p ~/new-dir1
RUN useradd -ms /bin/bash cloud_user
USER cloud_user
RUN mkdir -p ~/new-dir2
RUN mkdir -p /etc/myconf
RUN echo "Some config dada" >> /etc/myconf/my.conf
```

'/etc/myconf' permission denied error. 

```dockerfile
# after
FROM centos:latest
RUN mkdir -p ~/new-dir1
RUN useradd -ms /bin/bash cloud_user
RUN mkdir -p /etc/myconf
RUN echo "Some config dada" >> /etc/myconf/my.conf

USER cloud_user
RUN mkdir -p ~/new-dir2
```

Change the order of execution.

5. ##### Using the Volume Instruction

```dockerfile
FROM nginx:latest
Volume ["/usr/share/nginx/html/"]
```

​	Container Inspect
​	: -> get 'volume name'

​	`docker container inspect <container>`

​	Volume inspect 
​	: -> get 'Mountpoint'

​	`docker inspect volume <volume_name>`

​	Volume list

​	`sudo ls -la <mountpoint>`

6. ##### Entrypoint  vs Command

```dockerfile
FROM node
LABEL org.label-schema.version=v1.1
ENV NODE_ENV="production"
ENV PORT=3000

RUN mkdir -p /var/node
ADD src/ /var/node
WORKDIR /var/node
RUN npm install
EXPOSE $PORT
ENTRYPOINT ./bin/www
```

-> docker image build

-> `docker container run <image> echo <cmd>`

-> CMDs are created

7. ##### Using .dockerignore

   ```dockerfile
   # ignore files which end with '.md'
   */*.md
   # ignore all '.git' files
   */.git
   # ignore 'src/docs/* directory
   src/docs/
   # ignore 'tests' folder
   */tests/
   ```

   List of files which would be ignored when building a docker image.




### Deploy  with Flask

##### 1. Main Code with Flask

```python
''' app.py '''
app = Flask(__name__)
@app.route('/', methods=['GET'])
def valuePredict():

    input = [[15, 2, 152, 6, 1, 41, 1, 26, 15, 6]]
    output = get_prediction(input)
    return output

if __name__ == '__main__' :
    app.run(host='0.0.0.0', port=8888)
    
```

##### 2. Dockerfile Code

```dockerfile
# 1. Install base image
FROM python:3.11-slim-buster

# 2. Add requirements
COPY requirements.txt /requirements.txt

# 3. Install required python dependencies
RUN pip install --upgrade pip
RUN pip install -r requirements.txt

# 4. Copy source code in current directory
ADD . /app

# 5. Set working directory to '/app'
WORkDIR /app

# 6. Expose the port Flsk is running on
EXPOSE 8888

# 7. Run Flask
CMD ["flask", "run", "--host=0.0.0.0", "--port=8888"]
```

Command 'flask run' with host and port that are used in the main code.

##### 3. Docker Build Image

`Docker build -t <image_name>:v1 `

##### 4. Docker Run Image

`Docker run -rm -p 8888:8888 <image_name>:v1`

Port should be equal with flask port.

'rm' flag tells docker that the container should automatically be removed after closing docker.

