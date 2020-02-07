# Open CV Base base-cuda
# Build: docker image build --rm -t base-cuda .
#
# Includes
# - Ubuntu 18.04
# - Nvidia Cuda: 10.2

# Base On nvidia/cuda
# https://gitlab.com/nvidia/container-toolkit/nvidia-docker/-/tree/master
FROM nvidia/cuda:10.2-base-ubuntu18.04

# Check verion: docker container run --gpus all base-open-cv nvidia-smi
CMD nvidia-smi