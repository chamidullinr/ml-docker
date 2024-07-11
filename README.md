# Docker Images for Machine Learning and MLOps

This repository provides ready-to-run Docker images and Docker Compose configurations for applications essential in Machine Learning and MLOps workflows.

## Repository Structure

### Jupyterlab
Contains various JupyterLab configurations in Docker for both CPU and Nvidia GPU machines.
* [jupyterlab/jupyterlab-cpu](./jupyterlab/jupyterlab-cpu) - Run Jupyterlab with popular scientific packages. Based on [jupyter/scipy-notebook](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-scipy-notebook) Docker image.
* [jupyterlab/jupyterlab-cpu-multienv](./jupyterlab/jupyterlab-cpu-multienv) - Run JupyterLab with multiple `conda` environments, each preinstalled with packages for Machine Learning.
* [jupyterlab/jupyterlab-gpu](./jupyterlab/jupyterlab-gpu) - Run Jupyterlab with Nvidia GPU drivers. Based in [nvcr.io/nvidia/pytorch](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/pytorch) Docker image.  


### MLOps Infrastructure
Contains various Docker configurations for running DevOps and Monitoring applications useful for Machine Learning projects.
* [mlops-infra/gitlab](./mlops-infra/gitlab) - Set up a self-hosted GitLab instance for version control, CI/CD, and project management in ML workflows.
* [mlops-infra/prometheus-grafana](./mlops-infra/prometheus-grafana) - Deploy Prometheus for metrics collection and Grafana for visualization, enabling comprehensive monitoring of ML infrastructure and applications.
* [mlops-infra/pypi-server](./mlops-infra/pypi-server) - Run a private PyPI server to host and manage custom Python packages for your ML projects.
* [mlops-infra/nginx-certbot](./mlops-infra/nginx-certbot) - Configure Nginx as a reverse proxy with automatic SSL certificate management using Certbot, ensuring secure access to your ML services.

## Usage

Each subdirectory contains its own README with specific instructions for building and running the Docker images or Docker Compose configurations.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
