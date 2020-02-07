# Open CV Base
# Build: docker image build --rm -t base-opencv:3.7.6 .
#
# Includes
# - Ubuntu 18.04
# - Nvidia Cuda: 10.2
# - Python: 3.7.6
# - OpenCV: 4.1.0

# Base On nvidia/cuda
# https://gitlab.com/nvidia/container-toolkit/nvidia-docker/-/tree/master
FROM base-cuda-python:3.7.6
SHELL ["/bin/bash", "-c"]

## Update apt-get
RUN apt-get update -y && apt-get upgrade

## Install dependences packages
RUN apt-get install -y build-essential cmake unzip pkg-config \
    libjpeg-dev libpng-dev libtiff-dev \
    libjasper1  libjasper-dev \
    libavcodec-dev libavformat-dev libswscale-dev libv4l-dev \
    libxvidcore-dev libx264-dev \
    libgtk-3-dev \
    libatlas-base-dev gfortran \
    python3.7-dev

# Download & Install Open-CV
RUN mkdir /temp && cd /temp \
    && wget -O opencv.zip https://github.com/opencv/opencv/archive/4.2.0.zip \
    && unzip opencv.zip && mv opencv-4.0.0 opencv \
    && wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.2.0.zip \
    && unzip opencv_contrib.zip && mv opencv_contrib-4.0.0 opencv_contrib \
    && cd opencv && mkdir build && cd build \
    && cmake -D CMAKE_BUILD_TYPE=RELEASE \
              -D CMAKE_INSTALL_PREFIX=/usr/local \
              -D INSTALL_PYTHON_EXAMPLES=ON \
              -D INSTALL_C_EXAMPLES=OFF \
              -D OPENCV_ENABLE_NONFREE=ON \
              -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \
              -D PYTHON_EXECUTABLE=~/.virtualenvs/cv/bin/python \
              -D BUILD_EXAMPLES=ON .. \
    && make -j4 && make install && ldconfig

# Check verion: docker container run --gpus all base-cuda-python3
CMD python --version && pip --version
