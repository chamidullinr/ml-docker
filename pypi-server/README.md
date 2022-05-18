# PyPI Server Docker Image

The Docker image is an official image `pypiserver/pypiserver:latest`
which includes [pypiserver](https://github.com/pypiserver/pypiserver) -
a minimal PyPI compatible server for `pip` or `easy_install`.

The `docker-compose.yml` file in this repository is based on pypiserver docker-compose
[examples](https://github.com/pypiserver/pypiserver/blob/master/docker-compose.yml).
It includes configuration to authenticate with `.htpasswd` file
and to manage a package directory locally.

## Setup
Before starting the `pypiserver` create a `.htpasswd` file to authenticate access to 
the server and save the file to `./auth` directory.
The local `./auth` directory will be mounted
to a `/data/auth` directory in the image.

```bash
# install htpasswd tool
sudo apt-get install apache2-utils

# create .htpasswd file and include a user with a hashed password
mkdir ./auth
touch ./auth/.htpasswd
htpasswd -sc ./auth/.htpasswd [USERNAME]
```

Next, create a `./packages` directory to store packages locally.
The local `./packages` directory will be mounted
to a `/data/packages` directory in the image.
```bash
mkdir ./packages
```

## Run
Run the `docker-compose` command to start the image.
```bash
docker-compose up pypiserver
```

Note: It is possible to set actions (update, download, lost) 
that require authentication. See example: 
```yml
command: -P /data/auth/.htpasswd -a update,download,list /data/packages
```

## Upload `pip` Packages
Use [twine](https://twine.readthedocs.io/en/stable/) to upload `pip` packages.
The example below, uses authentication with selected username and password.
Change `http://localhost:8080/` to the server url
where the Docker image will be deployed.
```bash
# install twine package for uploading pip packages
pip install twine

# upload a package (.tar.gz or .whl) to the repository
twine upload \
  --repository-url http://localhost:8080/ \
  --username [USERNAME] \
  --password [PASSWORD] \
  [PACKAGE_FILE]
```

## Instal `pip` Packages


Change `http://localhost:8080/` to the server url
where the Docker image will be deployed.
```bash
pip install --extra-index-url http://localhost:8080/simple/ [PACKAGE_NAME]
```
