# Docker Image with Singularity and Docker Daemon pre-installed

The Docker image uses [Singularity](https://quay.io/repository/singularity/singularity) Alpine Linux image as a base
and installs Docker Daemon and [Singularity Python Recipes](https://singularityhub.github.io/singularity-cli/recipes) (`spython`) package.

The image enables use-cases like:
* Building Singularity image from Docker image from the host machine or the Docker Hub.
* Converting between Singularity and Docker recipes.

## Get Started
Run the following commands to build and start the image:
```bash
# build image
docker build -t chamidullinr/singularity-docker .

# create container and run bash in interactive mode in a terminal 
docker run -it --rm \
  --name=singularity-docker \
  --entrypoint=bash \
  --volume /var/run/docker.sock:/var/run/docker.sock \
  --volume [LOCAL_PATH]:/Data \
  chamidullinr/singularity-docker
```

Then run `singularity` or `spython` commands inside the container.

### Examples
Build a Singularity image from a Docker image `local/my_container:latest` and save it in `[LOCAL_PATH]` directory.
```bash
$ cd /Data
$ singularity build my_container.sif docker-daemon://local/my_container:latest
```

Or, create a Singularity file from `Dockerfile` stored in `[LOCAL_PATH]` directory.
```bash
$ cd /Data
$ spython recipe Dockerfile Singularity
```
