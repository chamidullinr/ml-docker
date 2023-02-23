FROM quay.io/singularity/singularity:v3.10.4

# install docker
RUN apk update && \
    apk add --update docker docker-compose openrc && \
    # enable docker daemon to start on boot time
    rc-update add docker boot

# install python, pip, and spython (Singularity Python Recipes)
ENV PYTHONUNBUFFERED=1
RUN apk add --update python3 && ln -sf python3 /usr/bin/python
RUN python3 -m ensurepip
RUN pip3 install --upgrade pip setuptools spython
