FROM nvidia/cuda:13.1.1-cudnn8-runtime-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive
WORKDIR /opt

# System dependencies
RUN apt-get update && apt-get install -y \
    wget \
    git \
    ffmpeg \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Install Miniconda
RUN wget -q https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
    && bash Miniconda3-latest-Linux-x86_64.sh -b -p /opt/conda \
    && rm Miniconda3-latest-Linux-x86_64.sh

ENV PATH=/opt/conda/bin:$PATH

# Copy environment definition
COPY environment.yml /opt/environment.yml

# Create conda environment
RUN conda env create -f /opt/environment.yml \
    && conda clean -afy

# Default shell activates conda env
SHELL ["conda", "run", "-n", "speciesnet", "/bin/bash", "-c"]

# Create data directory
RUN mkdir -p /data

# Health check (optional)
RUN python - <<EOF
import torch
print("CUDA available:", torch.cuda.is_available())
EOF

ENTRYPOINT ["conda", "run", "-n", "speciesnet", "python"]
