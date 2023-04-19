# Start from a core stack version
FROM jupyter/minimal-notebook:2023-02-13

ARG PYTHON_VERSION=3.9
ARG CREATE_R_CONDA_ENV=no
ARG INSTALL_GIT_LFS=no
ARG INSTALL_AWS_CLI=no

# Fix: https://github.com/hadolint/hadolint/wiki/DL4006
# Fix: https://github.com/koalaman/shellcheck/wiki/SC3014
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
WORKDIR "${HOME}"


# install ubuntu packages
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends \
    # for cython: https://cython.readthedocs.io/en/latest/src/quickstart/install.html
    build-essential \
    # for latex labels
    cm-super \
    dvipng \
    # for matplotlib anim
    ffmpeg \
    # other
    vim \
    curl \
    rsync \
    iputils-ping \
    traceroute \
    htop \
    ncdu


# install git lfs
RUN if [ "$INSTALL_GIT_LFS" = "yes" ]; then \
    curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | bash && \
    apt-get install git-lfs; \
    fi


# install AWS cli
RUN if [ "$INSTALL_AWS_CLI" = "yes" ]; then \
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    ./aws/install; \
    fi


# install conda packages
RUN mamba install --quiet --yes \
    'nb_conda_kernels' && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"


# install jupyterlab extensions
RUN pip install -U jupyterlab-git


# create conda environments and install python packages
COPY packages packages

# Python - scipy
RUN conda create --name scipy python="$PYTHON_VERSION" ipykernel ipywidgets
SHELL ["conda", "run", "-n", "scipy", "/bin/bash", "-c"]
RUN pip install -U -r packages/requirements_scipy.txt

# Python - pytorch (pytorch)
RUN conda create --name pytorch --clone scipy
SHELL ["conda", "run", "-n", "pytorch", "/bin/bash", "-c"]
RUN pip install -U -r packages/requirements_pytorch.txt

# R
RUN if [ "$INSTALL_AWS_CLI" = "yes" ]; then \
    apt-get install --yes --no-install-recommends \
        fonts-dejavu \
        unixodbc \
        unixodbc-dev \
        r-cran-rodbc \
        gfortran \
        gcc && \
    conda env create -f packages/env_R.yml; \
    fi

RUN rm -rf packages


# import matplotlib the first time to build the font cache.
ENV XDG_CACHE_HOME="/home/${NB_USER}/.cache/"
RUN MPLBACKEND=Agg python -c "import matplotlib.pyplot" && \
    fix-permissions "/home/${NB_USER}"


# add jupyter config
COPY jupyter/jupyter_config.json /home/jovyan/.jupyter/jupyter_config.json


# clean apt cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*


WORKDIR /
USER ${NB_UID}
