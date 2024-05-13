# Start from the GPU-enabled Jupyter Docker Stack with TensorFlow
FROM gitlab.ilabt.imec.be:4567/ilabt/gpu-docker-stacks/pytorch-notebook:cuda-11.8-2023-11-07

# Metadata
LABEL maintainer="thomas.dirven@hotmail.com" \
    project-name="Droid-SLAM" \
    com.droid-slam.version="0.0.1-alpha" \
    com.droid-slam.release-date="2023.01.20"

# Set arguments and env variables
ARG DEBIAN_FRONTEND=noninteractive

ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES graphics,utility,compute

#######################################
## Installing Extra software via apt ##
#######################################

# Apt requires root permissions, so switch to root
USER root

# Install base dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    software-properties-common \
    apt-utils \
    dbus-x11 \
    libglvnd0 \
    libgl1 \
    libglx0 \
    libegl1 \
    libxext6 \
    libx11-6 \
    libgl1-mesa-dev \
    libglew-dev \
    build-essential \
    libboost-all-dev \
    libsm6 \
    libxrender-dev \
    ninja-build \
    curl \
    git \
    ssh \
    unzip \
    vim \
    wget \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Fetch NVIDIA signing keys
RUN apt-key del 7fa2af80
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/3bf863cc.pub

# Set working directory
WORKDIR /home/jovyan

# Set user to root to avoid permission issues
USER root

# # Install Miniconda only if not already installed
# RUN if [ ! -d "/opt/conda" ]; then \
#     wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh && \
#     bash Miniconda3-latest-Linux-x86_64.sh -b -p /opt/conda && \
#     rm -f Miniconda3-latest-Linux-x86_64.sh; \
#     else echo "Miniconda already installed"; \
#     fi


ENV PATH="/opt/conda/bin:${PATH}"

# # Install Mamba
# RUN conda install -c conda-forge mamba

# # Install Droid SLAM dependencies using Mamba
# RUN mamba install -y python=3.9

# RUN mamba install -y pytorch==1.10.1 torchvision==0.11.2 torchaudio==0.10.1 cudatoolkit=11.3 -c pytorch -c conda-forge

# RUN pip install gdown matplotlib open3d opencv-python torch-scatter tensorboard scipy tqdm pyyaml

# RUN mamba install -c conda-forge suitesparse

# # Clone Droid-SLAM repository
# RUN git clone --recursive https://github.com/pytholic/DROID-SLAM.git /home/jovyan/DROID-SLAM

# # Install Droid-SLAM extensions
# RUN cd /home/jovyan/DROID-SLAM && \
#     python setup.py install

# # Cleanup
# RUN rm -rf /var/lib/apt/lists/*

# # Restore the correct user
# USER jovyan

# # Set entrypoint to bash
# ENTRYPOINT ["/bin/bash"]


# Install Mamba
RUN conda install -c conda-forge mamba

# Create a new conda environment with Python 3.9
RUN mamba create -n droidslam python=3.9 -y

# Activate the new environment and install dependencies
RUN echo "source activate droidslam" > ~/.bashrc

SHELL ["conda", "run", "-n", "droidslam", "/bin/bash", "-c"]

# Fix permissions for .cache
RUN mkdir -p /home/jovyan/.cache/pip && chmod -R 777 /home/jovyan/.cache/pip

# Install Python packages using pip
RUN mamba install -y pytorch==1.10.1 torchvision==0.11.2 torchaudio==0.10.1 cudatoolkit=11.3 -c pytorch -c conda-forge

RUN /opt/conda/envs/droidslam/bin/pip install --no-cache-dir gdown matplotlib open3d opencv-python torch-scatter tensorboard scipy tqdm pyyaml

# Install remaining Python packages using pip
# RUN pip install --no-cache-dir gdown matplotlib open3d opencv-python tensorboard scipy tqdm pyyaml

# Install torch-scatter separately after torch is available
# RUN pip install torch-scatter

# Install torch-scatter using conda
# RUN mamba install -c conda-forge torch-scatter

RUN mamba install -c conda-forge suitesparse

# Clone Droid-SLAM repository
RUN git clone --recursive https://github.com/pytholic/DROID-SLAM.git /home/jovyan/DROID-SLAM

# Install Droid-SLAM extensions
RUN cd /home/jovyan/DROID-SLAM && \
    python setup.py install

# Register the environment as a Jupyter kernel
RUN pip install ipykernel && \
    python -m ipykernel install --name droidslam --display-name "Python 3.9 (droidslam)"

# Cleanup
RUN rm -rf /var/lib/apt/lists/*

# Restore the correct user
USER jovyan

# Set entrypoint to bash
ENTRYPOINT ["/bin/bash"]
