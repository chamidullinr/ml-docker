# Jupyterlab Docker Image for Machine Learning on GPU

The Docker image inherits from [nvcr.io/nvidia/pytorch](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/pytorch)
and includes `pip` packages related to deep learning.

## Get Started
Modify configuration in `docker-compose.yml`.
* Bind volumes.
* Modify CPU and GPU resource limits.

Optionally, add desired packages to requirements files in `packages` directory.

Then, run `docker-compose`.
```bash
docker compose build
docker compose up -d
```
