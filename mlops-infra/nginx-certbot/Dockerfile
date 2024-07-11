FROM nginx:1.22.0

# install python
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends \
    python3 \
    python3-venv \
    libaugeas0 && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# create python environment and install pip
RUN python3 -m venv /opt/certbot/ && \
    /opt/certbot/bin/pip install --upgrade pip

# install certbot
RUN /opt/certbot/bin/pip install certbot certbot-nginx && \
    ln -s /opt/certbot/bin/certbot /usr/bin/certbot

# CMD ["nginx", "-g", "daemon off;"]
