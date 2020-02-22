# Python Base
# Build: docker image build --rm -t base-cuda-python:3.7.6 .
#
# Includes
# - Ubuntu 18.04
# - Nvidia Cuda: 10.2
# - Python: 3.7.6

# Base On nvidia/cuda
# https://gitlab.com/nvidia/container-toolkit/nvidia-docker/-/tree/master
FROM base-cuda:10.2
SHELL ["/bin/bash", "-c"]

# http://bugs.python.org/issue19846
# > At the moment, setting "LANG=C" on a Linux system *fundamentally breaks Python 3*, and that's not OK.
ENV LANG C.UTF-8
ENV DEBIAN_FRONTEND=noninteractive 

# Install prerequisites package
RUN apt-get update && apt-get upgrade -y \
    && apt-get install -y --no-install-recommends \
          software-properties-common \
          build-essential gcc make cmake wget tar tzdata unzip \
          zlib1g-dev libffi-dev \
          libssl-dev libncurses5-dev libsqlite3-dev libreadline-dev libtk8.5 \
          libgdm-dev libdb4o-cil-dev libpcap-dev

# Download source & Build & Install Python 3.7.6
RUN mkdir /temp \
    && cd /temp \
    && wget https://www.python.org/ftp/python/3.7.6/Python-3.7.6.tgz \
    && tar -xvf Python-3.7.6.tgz \
    && cd Python-3.7.6 \
    && ./configure \
    && make \
    &&  make install \
    && rm -rf /temp

# Install PIP and dependencers
RUN apt-get install -y python-dev python3-dev python3-pip python3-venv \
    && apt-get clean \
    && rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/*

# Create base virtual environments
RUN rm -rf  /base \
    && python3 -m venv base \
    && source /base/bin/activate \
    && python --version && pip install --upgrade pip \
    && pip install wheel numpy scipy matplotlib scikit-image scikit-learn ipython dlib

# Auto activate base for bash
RUN echo -e "\n # Auto activate base \nsource /base/bin/activate\n" >> ~/.bashrc

# Check verion: docker container run --gpus all base-cuda-python:3.7.6 nvidia-smi
CMD python --version && pip --version
