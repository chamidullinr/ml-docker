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
    # other
    vim && \
    htop && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# USER ${NB_UID}
RUN mamba install --quiet --yes \
    'nb_conda_kernels' && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# create conda environment and install python packages
COPY packages packages
# scipy
RUN conda env create -f packages/env_scipy.yml
SHELL ["conda","run","-n","conda_scipy","/bin/bash","-c"]
RUN pip install -U -r packages/requirements_scipy.txt

# Import matplotlib the first time to build the font cache.
ENV XDG_CACHE_HOME="/home/${NB_USER}/.cache/"
RUN MPLBACKEND=Agg python -c "import matplotlib.pyplot" && \
    fix-permissions "/home/${NB_USER}"

# pytorch for cpu
RUN conda env create -f packages/env_torch.yml
SHELL ["conda","run","-n","conda_torch","/bin/bash","-c"]
RUN pip install -U -r packages/requirements_torch.txt

RUN rm -rf packages

# add jupyter config
COPY jupyter/jupyter_config.json /home/jovyan/.jupyter/jupyter_config.json

USER ${NB_UID}

WORKDIR "${HOME}"