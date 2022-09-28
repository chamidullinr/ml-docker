# GitLab Docker Setup
This file contains instructions to set up and run GitLab docker image using [sameersbn/docker-gitlab](https://github.com/sameersbn/docker-gitlab) repo.

## Getting Started
Follow the steps to set up GitLab docker:

1. Pull gitlab docker image.
    ```bash
    docker pull sameersbn/gitlab:15.3.2
    ```
2. Load `docker-compose.yml` file.
    ```bash
    wget https://raw.githubusercontent.com/sameersbn/docker-gitlab/master/docker-compose.yml
    ```
3. Generate random strings that are at least 64 characters long
for environment variables in `docker-compose.yml`: `GITLAB_SECRETS_OTP_KEY_BASE`, `GITLAB_SECRETS_DB_KEY_BASE`, and `GITLAB_SECRETS_SECRET_KEY_BASE`.
    ```bash
    pwgen -Bsv1 64
    ```
   Do not lose these secret keys.
   See [docker-gitlab](https://github.com/sameersbn/docker-gitlab#quick-start) for more information.
4. Setup `SMTP` mail configuration to allow GitLab to send emails. Set the following variables:
    ```yaml
    SMTP_ENABLED=true  # not necessary, defaults to true if SMTP_USER is defined
    SMTP_USER=USER@gmail.com
    SMTP_PASS=PASSWORD
    SMTP_DOMAIN=www.gmail.com
    SMTP_HOST=smtp.gmail.com
    SMTP_PORT=587
    SMTP_STARTTLS=true
    ```
5. (optional) Include additional configuration `docker-compose.yml`. See section [Optional Configuration](#optional-configuration).
6. Start GitLab.
    ```bash
    docker-compose up
    ```
7. Go to http://localhost:10080 or http://localhost:443 in the browser and set a password for the root user account.


## Optional Configuration
Optional configuration in `docker-compose.yml`:

**HTTPS/SSL** - Set the `GITLAB_HTTPS` option to `true` if using load balancer like nginx to set SSL certificates,
Additionally, set the `SSL_SELF_SIGNED` option to `true` if using self signed SSL certificates,
and to `false` if using Certificate Authority (CA).

**OmniAuth Integration** - GitLab uses [OmniAuth](https://rubygems.org/gems/omniauth/) framework to allow
users to sign in to GitLab by using their credentials from Google, GitHub, and other services.
* Google
  1. Register GitLab application with Google and generate a Client ID and a secret. See [more](https://docs.gitlab.com/ee/integration/google.html).
  2. Set `OAUTH_GOOGLE_API_KEY` and `OAUTH_GOOGLE_APP_SECRET` environment variables.
* GitHub
  1. Register GitLab application with GitHub and generate a Client ID and a secret. See [more](https://docs.gitlab.com/ee/integration/github.html).
  2. Set `OAUTH_GITHUB_API_KEY` and `OAUTH_GITHUB_APP_SECRET` environment variables.


## Other Components Configuration
The section describes how to set up useful components like CI/CD Runners or Docker Registry.

### Runners for CI/CD
General instructions (includes Linux, macOS, Windows, Docker, and Kubernetes):
1. Go to http://localhost:8080/admin/runners
2. See instructions on **Register an instance runner** -> **Show runner installation and registration instructions**.

Instructions for using a Docker image:
1. Run gitlab runner using `gitlab/gitlab-runner:latest` docker image - [docs](https://docs.gitlab.com/runner/install/docker.html#option-1-use-local-system-volume-mounts-to-start-the-runner-container).
   ```bash
   docker run -d --name gitlab-runner --restart always \
   -v /srv/gitlab-runner/config:/etc/gitlab-runner \
   -v /var/run/docker.sock:/var/run/docker.sock \
   gitlab/gitlab-runner:latest
   ```
2. Register a runner - [docs](https://docs.gitlab.com/runner/register/index.html#docker).
    ```bash
    docker run --rm -it \
      -v /srv/gitlab-runner/config:/etc/gitlab-runner \
      gitlab/gitlab-runner register
    ```
   
Known issues:
* Error when running `docker login ...`  in a CI/CD job:
`error during connect: Post "http://docker:2375/v1.24/auth": dial tcp: lookup docker on 147.228.3.3:53: no such host`.
See solution in [techoverflow](https://techoverflow.net/2021/01/12/how-to-fix-gitlab-ci-error-during-connect-post-http-docker2375-v1-40-auth-dial-tcp-lookup-docker-on-no-such-host/).
   
### Git LFS
GitLab LFS functionality is turned on by default on `sameersbn/docker-gitlab` docker compose.

The environment variables for setting GitLab LFS are the following:
* `GITLAB_LFS_ENABLED=true` (default)
* `GITLAB_LFS_OBJECTS_DIR=$GITLAB_SHARED_DIR/lfs-objects`  (default)

where `GITLAB_SHARED_DIR=/home/git/data/shared` is stored in a persistent storage volume `gitlab-data`.

### Docker Registry
Docker Registry is a storage and delivery system for Docker images.
    See [Docker docs](https://docs.docker.com/registry/introduction/) and [GitLab docs](https://gitlab.com/help/administration/packages/container_registry.md) for more information.

Instructions to set up Docker Registry in GitLab, see details in [sameersbn/docker-gitlab](https://github.com/sameersbn/docker-gitlab/blob/master/docs/container_registry.md) repo:
1. Update `docker-compose.yml`: add `registry` container and update `gitlab` container environment and volumes.
2. Generate certificate in `./certs` directory.
    ```bash
    cd certs
    # Generate a random password password_file used in the next commands
    openssl rand -hex -out password_file 32
    # Create a PKCS#10 certificate request
    openssl req -new -passout file:password_file -newkey rsa:4096 -batch > registry.csr
    # Convert RSA key
    openssl rsa -passin file:password_file -in privkey.pem -out registry.key
    # Generate certificate
    openssl x509 -in registry.csr -out registry.crt -req -signkey registry.key -days 10000
    ```
3. Create Nginx configuration for `registry.example.com`. HTTPS functionality can be added using Certbot.
4. Run `docker-compose up -d`. This should (re)create containers `gitlab` and `gitlab-registry`.
