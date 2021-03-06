`docker run -d -p 80:80 docker/getting-started`<br/>
Flags:
* `-d` detached mode (in the background)
* `-p 80:80` map port 80 of the host to port 80 in the container
* `docker/getting-started` image to use

`docker run --rm -ti --name custom_name ubuntu:latest bash`
* `--rm` delete the container after finish
* `-ti` terminal interactive
* `--name` container name user-especified
* `ubuntu-latest` parent image
* `bash` we can use the bash

# Attach to container
`docker attach container-name`
deatach
`ctl + p, ctl + q`

# Logs
`docker logs container-name`

# Kill container
```
docker kill container-name
docker rm container-name
```
# Container networking
* Expose ports
* Private networks to connect between containers


# Create an image
```
docker commit container-id
docker tag new-id tag-new-container
```

# Dockerfile
```
FROM node:12-alpine <- Start from 12-alpine image
WORKDIR /app 
COPY . .
RUN yarn install --production <- Install dependecies using yarn
CMD["node", "src/index.js"]
```

The container is build using
`docker build -t tag-name .`
`-t` tag the image with `tag-name`
`.` at the end tell docket to look for Dockerfile in tne current directory

# Start container
`docker run -dp 3000:3000 getting-started`
open browser on `localhost:3000`

# Stop and remove container
List containers `docker container ls` or `docker ps -a`

Stop container `docker stop <container-name>`

Remove container `docker rm <container-name>`

Stop and remove in one step `docker rm -f <container-name>`
