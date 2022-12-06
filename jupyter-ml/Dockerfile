# Start from a core stack version
FROM jupyter/minimal-notebook

# Fix: https://github.com/hadolint/hadolint/wiki/DL4006
# Fix: https://github.com/koalaman/shellcheck/wiki/SC3014
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends \
    # for cython: https://cython.readthedocs.io/en/latest/src/quickstart/install.html
    build-essential \
    # for latex labels
    cm-super \
    dvipng \
    # for matplotlib anim
    ffmpeg \
    # for R
    fonts-dejavu \
    unixodbc \
    unixodbc-dev \
    r-cran-rodbc \
    gfortran \
    gcc \
    # other
    vim \
    curl \
    htop && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

#USER ${NB_UID}
WORKDIR "${HOME}"

RUN mamba install --quiet --yes \
    'nb_conda_kernels' && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# create conda environment and install python packages
COPY packages packages

# Python - scipy
RUN conda env create -n scipy python=3.8 ipykernel ipywidgets
SHELL ["conda","run","-n","scipy","/bin/bash","-c"]
RUN pip install -U -r packages/requirements_scipy.txt

# Python - computer vision (cv)
RUN conda env create -n cv python=3.8 ipykernel ipywidgets
SHELL ["conda","run","-n","cv","/bin/bash","-c"]
RUN pip install -U -r packages/requirements_scipy.txt -r packages/requirements_cv.txt

# Python - natural language processing (nlp)
RUN conda env create -n nlp python=3.8 ipykernel ipywidgets
SHELL ["conda","run","-n","nlp","/bin/bash","-c"]
RUN pip install -U -r packages/requirements_scipy.txt -r packages/requirements_nlp.txt

# R
RUN conda env create -f packages/env_R.yml

RUN rm -rf packages

# import matplotlib the first time to build the font cache.
ENV XDG_CACHE_HOME="/home/${NB_USER}/.cache/"
RUN MPLBACKEND=Agg python -c "import matplotlib.pyplot" && \
    fix-permissions "/home/${NB_USER}"

# add jupyter config
COPY jupyter/jupyter_config.json /home/jovyan/.jupyter/jupyter_config.json

# install AWS cli
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    ./aws/install

WORKDIR /
USER ${NB_UID}
