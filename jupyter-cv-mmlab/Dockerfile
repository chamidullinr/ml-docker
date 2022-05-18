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
    vim \
    htop && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# install packages
COPY requirements.txt requirements.txt
RUN pip install -U -r requirements.txt
RUN rm requirements.txt

# Import matplotlib the first time to build the font cache.
ENV XDG_CACHE_HOME="/home/${NB_USER}/.cache/"
RUN MPLBACKEND=Agg python -c "import matplotlib.pyplot" && \
    fix-permissions "/home/${NB_USER}"

# install open-mmlab packages
RUN git clone https://github.com/open-mmlab/mmcv.git && \
    cd mmcv && \
    pip install -r requirements/optional.txt && \
    MMCV_WITH_OPS=1 pip install -e .
RUN cd ..

RUN git clone https://github.com/picekl/mmdetection && \
    cd mmdetection && \
    pip install -r requirements/build.txt && \
    pip install -v -e .
RUN cd ..

RUN git clone https://github.com/open-mmlab/mmpose.git && \
    cd mmpose && \
    pip install -r requirements.txt && \
    pip install -v -e . \
RUN cd ..

USER ${NB_UID}

WORKDIR "${HOME}"
