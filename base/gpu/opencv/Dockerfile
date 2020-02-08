# Open CV Base
# Build: docker image build --rm -t base-opencv:4.2.0 .
#
# Includes
# - Ubuntu 18.04
# - Nvidia Cuda: 10.2
# - Python: 3.7.6
# - OpenCV: 4.2.0

# Base On nvidia/cuda
# https://gitlab.com/nvidia/container-toolkit/nvidia-docker/-/tree/master
FROM base-cuda-python:3.7.6
SHELL ["/bin/bash", "-c"]

## Update apt-get
RUN apt-get update -y && apt-get upgrade

## Install dependences packages
RUN apt-get install -y build-essential cmake unzip pkg-config \
                       libjpeg-dev libpng-dev libtiff-dev \
                       libavcodec-dev libavformat-dev libswscale-dev libv4l-dev \
                       libxvidcore-dev libx264-dev \
                       libgtk-3-dev \
                       libatlas-base-dev gfortran

# Download & Install Open-CV
RUN source /base/bin/activate && python --version \
    && mkdir /temp && cd /temp \
    && wget -O opencv.zip https://github.com/opencv/opencv/archive/4.2.0.zip \
    && unzip opencv.zip && mv opencv-4.2.0 opencv \
    && wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.2.0.zip \
    && unzip opencv_contrib.zip && mv opencv_contrib-4.2.0 opencv_contrib \
    && cd opencv && mkdir build && cd build \
    && cmake -D CMAKE_BUILD_TYPE=RELEASE \
              -D CMAKE_INSTALL_PREFIX=/usr/local \
              -D INSTALL_PYTHON_EXAMPLES=ON \
              -D INSTALL_C_EXAMPLES=OFF \
              -D OPENCV_ENABLE_NONFREE=ON \
              -D OPENCV_EXTRA_MODULES_PATH=/temp/opencv_contrib/modules \
              -D PYTHON_EXECUTABLE=/base/bin/python \
              -D BUILD_EXAMPLES=ON .. \
    && make -j4 && make install && ldconfig \
    && cat /temp/opencv/build/CMakeFiles/CMakeError.log \
    && rm -rf /temp

# Check verion: docker container run --gpus all base-cuda-python3
CMD python --version && pip --version
