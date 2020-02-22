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

## Check Python version && PIP version
RUN source /base/bin/activate && python --version && pip --version

## Update apt-get && Install dependences packages
RUN apt-get update -y && apt-get upgrade -y \
    && apt-get install -y software-properties-common \
    && add-apt-repository "deb http://security.ubuntu.com/ubuntu xenial-security main" \
    && apt-get -y update \
    && apt-get install -y libjasper1 libtiff-dev libavcodec-dev libavformat-dev libswscale-dev libdc1394-22-dev libxine2-dev libv4l-dev \
    && cd /usr/include/linux && ln -s -f ../libv4l1-videodev.h videodev.h \
    && apt-get install -y   build-essential checkinstall cmake pkg-config yasm git gfortran libjpeg8-dev libpng-dev \
                            libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev \
                            libgtk2.0-dev libtbb-dev qt5-default \
                            libatlas-base-dev \
                            libfaac-dev libmp3lame-dev libtheora-dev \
                            libvorbis-dev libxvidcore-dev \
                            libopencore-amrnb-dev libopencore-amrwb-dev \
                            libavresample-dev x264 v4l-utils \
                            libprotobuf-dev protobuf-compiler \
                            libgoogle-glog-dev libgflags-dev \
                            libgphoto2-dev libeigen3-dev libhdf5-dev doxygen

# # Download & Install Open-CV
RUN mkdir /temp && cd /temp \
    && wget -O opencv.zip https://github.com/opencv/opencv/archive/4.2.0.zip \
    && unzip opencv.zip && mv opencv-4.2.0 opencv \
    && wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.2.0.zip \
    && unzip opencv_contrib.zip && mv opencv_contrib-4.2.0 opencv_contrib \
    && cd opencv && mkdir build && cd build \
    && cmake -D CMAKE_BUILD_TYPE=RELEASE \
            -D CMAKE_INSTALL_PREFIX=/usr/local \
            -D INSTALL_C_EXAMPLES=OFF \
            -D INSTALL_PYTHON_EXAMPLES=ON \
            -D WITH_TBB=ON \
            -D WITH_V4L=ON \
            -D PYTHON_EXECUTABLE=/base/bin/python \
            -D OPENCV_PYTHON3_INSTALL_PATH=/base/lib/python3.7/site-packages/ \
            -D WITH_QT=ON \
            -D WITH_OPENGL=ON \
            -D OPENCV_EXTRA_MODULES_PATH=/temp/opencv_contrib/modules \
            -D BUILD_EXAMPLES=ON .. \
    && make -j4 && make install \
    && rm -rf /temp

## Install Python OpenCV
RUN source /base/bin/activate \
    && pip install opencv-python \
    && pip install opencv-contrib-python

# Check verion: docker container run --gpus all base-opencv:4.2.0 nvidia-smi
CMD python --version && pip --version
