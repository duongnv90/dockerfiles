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

# Install prerequisites package
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
          software-properties-common \
          gcc \
          make \
          zlib1g-dev \
          libffi-dev \
          wget \
          tar 

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
RUN apt-get install -y build-essential libssl-dev libffi-dev python-dev python3-pip python3-venv

# Confirm version
RUN python3 --version && pip3 --version

# Create base virtual environments
RUN rm -rf  /base && python3 -m venv base \
    && source /base/bin/activate \
    && python --version

# Auto activate base for bash
RUN echo -e "\n # Auto activate base \n source /base/bin/activate " >> ~/.bashrc

# Check verion: docker container run --gpus all base-cuda-python3
CMD python --version && pip --version
