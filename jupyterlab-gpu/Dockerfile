FROM nvcr.io/nvidia/pytorch:23.10-py3

# install system packages
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update --yes && \
    # - apt-get upgrade is run to patch known vulnerabilities in apt-get packages as
    #   the ubuntu base image is rebuilt too seldom sometimes (less than once a month)
    apt-get upgrade --yes && \
    apt-get install --yes --no-install-recommends \
    # main
    sudo \
    unattended-upgrades \
    # useful tools
    mc \
    git \
    vim \
    curl \
    htop \
    wget \
    ncdu

# install git lfs
RUN curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | bash && \
    apt-get install git-lfs

# automatically install latest security (and other) updates
RUN unattended-upgrade

# update pip
RUN python -m pip install --no-cache-dir --upgrade pip build setuptools

# install python packages
RUN pip install --no-cache-dir --upgrade \
    # base
    numpy \
    scipy \
    pandas \
    openpyxl \
    matplotlib \
    seaborn \
    plotly \
    scikit-learn \
    Pillow \
    joblib \
    tqdm \
    ipywidgets \
    python-dotenv \
    # nlp
    nltk \
    transformers \
    trl \
    peft \
    accelerate \
    datasets \
    SentencePiece \
    alpaca_eval \
    bitsandbytes \
    # computer vision
    albumentations \
    timm \
    ultralytics \
    # mlops
    mlflow \
    wandb \
    loguru \
    # code quality
    black[jupyter] \
    isort \
    flake8 \
    flake8-docstrings \
    # jupyterlab
    ipywidgets \
    jupyterlab==3.6.6 \
    jupyter-dash \
    jupyterlab-git

# clean apt-get cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# install node
RUN curl -sL https://deb.nodesource.com/setup_18.x | bash - && \
    apt-get install --yes nodejs

# build Jupyter Lab with newly installed extensions.
RUN NODE_OPTIONS=--max-old-space-size=8192 jupyter lab build

# define command to run when the container starts
WORKDIR "/"
CMD jupyter lab --ip 0.0.0.0 --port 8889 --allow-root
