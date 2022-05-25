# Jupyter Docker Image for Machine Learning

The Docker image definition inherits from [jupyter-minimal-notebook](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-minimal-notebook)
and includes conda environments:
* **Conda Scipy** with `Python=3.8` and packages like `numpy`, `pandas`, `matplotlib`, etc.
* **Conda PyTorch** with `Python=3.8` and packages like `torch=1.10.1` and `torchvision=0.11.2`

Run the following commands to build and start the image:
```bash
docker build -t chamidullinr/jupyter-ml .

docker run -p 8888:8888 --name=jupyter-ml chamidullinr/jupyter-ml 
```

Run the following command inside Docker image to create a new conda environment:
```bash
conda create -n <NAME> python=<VERSION> ipykernel
```