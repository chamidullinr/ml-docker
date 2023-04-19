# Jupyterlab Docker Image for Machine Learning on CPU

The Docker image inherits from [jupyter-minimal-notebook](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-minimal-notebook)
and includes various `conda` environments useful for ML, CV, and NLP:
* **scipy** with `python` packages like `numpy`, `scipy`, `scikit-lears`, `pandas`, etc.
* **pytorch** with `python` packages from **scipy** and `pytorch` for CPU. 
* **cv** with `python` packages from **pytorch** and `timm`, `albumentations`, `opencv`, etc.
* **nlp** with `python` packages from **pytorch** and `transformers`, `datasets`.

Optionally, the `Dockerfile` enables to install `Git LFS` and `AWS CLI` tools.

The image is developed and tested for CPU-based environments.
For systems with GPU it is recommended to use Nvidia NGC images like `nvcr.io/nvidia/pytorch` as a base. 

## Get Started
Modify configuration in `docker-compose.yml`.
* Bind volumes.
* Modify resource limits, default values are `cpus=2.0` and `memory=12g`.
* Set build arguments like `PYTHON_VERSION`, `CREATE_R_CONDA_ENV=yes/no`, `INSTALL_GIT_LFS=yes/no`, `INSTALL_AWS_CLI=yes/no`.

Optionally, add desired packages to requirements files in `packages` directory.

Then, run `docker-compose`.
```bash
docker-compose build
docker-compose up -d
```

### Create Conda Environments
Run the following command inside Docker image (e.g. in Jupyterlab terminal) to create a new conda environment:
```bash
conda create -n <NAME> python=<VERSION> ipykernel
```
