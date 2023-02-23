# Jupyterlab Docker Image with OpenMMLab dependencies

The Docker image definition inherits from [jupyter-minimal-notebook](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-minimal-notebook)
and includes Computer Vision packages like:
* MMCV, MMDetection, MMPose
* OpenCV, Pillow
* PyTorch, PyTorch Image Models (`timm`), Albumentations

Run the following commands to build and start the image:
```bash
docker build -t chamidullinr/jupyter-cv-mmlab .

docker run -p 8888:8888 --name=jupyter-cv-mmlab chamidullinr/jupyter-cv-mmlab
```
