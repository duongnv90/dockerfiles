# Open CV Base base-cuda
# Build: docker image build --rm -t base-gpu-python3 .
#
# Includes
# - Ubuntu 18.04
# - Nvidia Cuda: 10.2
# - Python: 3.7.6

# Base On nvidia/cuda
# https://gitlab.com/nvidia/container-toolkit/nvidia-docker/-/tree/master
FROM base-cuda

# ensure local python is preferred over distribution python
ENV PATH /usr/local/bin:$PATH

# http://bugs.python.org/issue19846
# > At the moment, setting "LANG=C" on a Linux system *fundamentally breaks Python 3*, and that's not OK.
ENV LANG C.UTF-8

# extra dependencies (over what buildpack-deps already includes)
RUN apt-get update && apt-get install -y --no-install-recommends \
		build-essential \
		zlib1g-dev \
		libncurses5-dev \
		libgdbm-dev \
		libnss3-dev \
		libssl-dev \
		libreadline-dev \
		libffi-dev \
		wget \
	&& rm -rf /var/lib/apt/lists/*

# Python version
ENV PYTHON_VERSION 3.7.6

# Download python source
RUN wget https://www.python.org/ftp/python/3.7.6/Python-3.7.6.tgz \
    && tar -xf Python-3.7.6.tgz \
    && cd Python-3.7.6 \
    && ./configure --enable-optimizations \
    && make altinstall \
    && python3 --version

# Check verion: docker container run --gpus all base-gpu-python3
CMD python3 --version