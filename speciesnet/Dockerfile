FROM nvidia/cuda:12.2.2-cudnn8-runtime-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive
WORKDIR /opt

# System deps
RUN apt-get update && apt-get install -y \
    wget \
    git \
    ffmpeg \
    ca-certificates \
    python3 \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# Upgrade pip
RUN python3 -m pip install --upgrade pip

# Install PyTorch with CUDA 12.2 support
RUN pip install \
    torch torchvision \
    --index-url https://download.pytorch.org/whl/cu122

# Install cameratrappAI
RUN pip install git+https://github.com/google/cameratrapai.git

# Create data directory
RUN mkdir -p /data

# Sanity check
RUN python3 - <<EOF
import torch
print("CUDA available:", torch.cuda.is_available())
print("CUDA version:", torch.version.cuda)
print("GPU:", torch.cuda.get_device_name(0))
EOF

ENTRYPOINT ["python3"]
