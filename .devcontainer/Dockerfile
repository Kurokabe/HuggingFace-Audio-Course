FROM pytorch/pytorch:2.0.1-cuda11.7-cudnn8-runtime

RUN apt update -y
RUN apt install git -y
RUN apt install htop -y
RUN apt install gcc -y
RUN apt install curl -y
RUN curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh

RUN apt-get install git-lfs


WORKDIR /build
RUN python3 -m pip install --upgrade pip

ARG USERNAME=dev
ARG USER_UID=1002
ARG USER_GID=$USER_UID

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    #
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

RUN apt-get update \
    && apt-get install -y build-essential \
    && apt-get install -y wget \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# # Install miniconda
# ENV CONDA_DIR /opt/conda
# RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh && \
#     /bin/bash ~/miniconda.sh -b -p /opt/conda

# # Put conda in path so we can use conda activate
# ENV PATH=$CONDA_DIR/bin:$PATH


# ********************************************************
# * Anything else you want to do like clean up goes here *
# ********************************************************

# Install python dependencies
COPY requirements.txt .
RUN python3 -m pip install -r requirements.txt

RUN echo export PYOPENGL_PLATFORM=x11 >> ~/.bashrc

# [Optional] Set the default user. Omit if you want to keep the default as root.
USER $USERNAME


