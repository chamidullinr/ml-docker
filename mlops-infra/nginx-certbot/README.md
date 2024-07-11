# Nginx with Certbot Configuration

The Docker image inherits from [Nginx](https://hub.docker.com/_/nginx) and includes [Certbot](https://certbot.eff.org/) installation.
Certbot is an ACME client recommended by Let's Encrypt certificate authority that automates certificate issuance and installation with no downtime.
In other words, Certbot sets up HTTPS on websites.

Deployment steps:
1. Deploy Nginx container with Certbot (pip) installation.
    ```bash
    docker-compose up -d
    ```
2. Connect to the Nginx container terminal.
   ```bash
   docker exec -it nginx bash
   ```
3. Run Certbot. The program will ask which domains it should set up HTTPS for.
   ```bash
   certbot --nginx
   ```
4. Set automatic renewal.
   ```bash
   echo "0 0,12 * * * root /opt/certbot/bin/python -c 'import random; import time; time.sleep(random.random() * 3600)' && certbot renew -q" | tee -a /etc/crontab > /dev/null
   ```
