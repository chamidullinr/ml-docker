# Jupyter Docker Image for Machine Learning

The Docker image inherits from [jupyter-minimal-notebook](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-minimal-notebook)
and includes conda environments:
* **cv** with `Python=3.8` and packages like `pytorch`, `timm`, `opencv`, etc.
* **nlp** with `Python=3.8` and packages like `pytorch`, `transformers`, and `datasets`.

## Docker
Run the following commands to build and start the image:
```bash
docker build -t chamidullinr/jupyter-ml .
docker run -p 8888:8888 --name=jupyter-ml chamidullinr/jupyter-ml 
```

Optionally, start the image with additional parameters:
```bash
docker run \
	-p 8888:8888 \
	-v ~/Documents:/home/jovyan/work \
	-u root \
	-e GRANT_SUDO=yes \
	--cpus="2.0" \
	--memory="12g" \
	--name jupyter-ml \
	--detach \
	chamidullinr/jupyter-ml
```
The parameters are:
* `--publish`, `-p` - Publish a container's port(s) to the host
* `--volume`, `-v` - Bind mount a volume
  * Use `//c/Users/username/Documents:/home/jovyan/work` for Windows host.
* `--user`, `-u` - Username or UID (format: <name|uid>[:<group|gid>])
* `--env`, `-e` - Set environment variables
  * Parameter `GRANT_SUDO=yes` is Jupyter Docker specific to use root user, e.g. for `sudo apt install`.
* `--cpus` - Number of CPUs
* `--memory`, `-m` - Memory limit
* `--detach`, `-d` - Run container in background and print container ID

## Docker Compose
Alternatively, build and start the image using `docker-compose`:
```bash
docker-compose build
docker-compose up -d
```

## Conda Environment
Run the following command inside Docker image to create a new conda environment:
```bash
conda create -n <NAME> python=<VERSION> ipykernel
```
