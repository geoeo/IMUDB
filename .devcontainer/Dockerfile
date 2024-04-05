#FROM pytorchlightning/pytorch_lightning:base-cuda-py3.6-torch1.8
FROM nvidia/cuda:11.1.1-cudnn8-devel-ubuntu18.04 AS build

RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/3bf863cc.pub 249
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/7fa2af80.pub 80

RUN apt-get update ##[edited]

# INSTALL SYSTEM DEPS + OpenCV Deps.
RUN apt-get update && apt-get install -y \
        build-essential \
        python3-pip \
        pkg-config \
        git \
        wget \
        curl \
        unzip \
        sudo \
        apt-utils \
        lsb-release \
        libglew-dev \
        gdb \
        libgtk2.0-dev

RUN apt-get install ffmpeg libsm6 libxext6  -y

WORKDIR /root

COPY ./ /root/imudb

WORKDIR /root/imudb

#Install miniconda
ENV CONDA_DIR /opt/conda
RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh && \
/bin/bash ~/miniconda.sh -b -p /opt/conda
# Put conda in path so we can use conda activate
ENV PATH=$CONDA_DIR/bin:$PATH

RUN conda create -y -n imudb python=3.8
RUN conda run -n imudb --no-capture-output pip install \
    pytorch-lightning==1.4.2 torchmetrics==0.6.0 torch==1.9.1+cu111 torchvision==0.10.1+cu111 torchaudio==0.9.1 -f https://download.pytorch.org/whl/torch_stable.html
RUN conda run -n imudb --no-capture-output pip install catkin_pkg
RUN conda run -n imudb --no-capture-output pip install git+https://github.com/eric-wieser/ros_numpy.git
RUN conda run -n imudb --no-capture-output pip install -r requirements.txt
RUN conda run -n imudb --no-capture-output pip install --extra-index-url https://rospypi.github.io/simple/ rosbag
RUN conda init bash
