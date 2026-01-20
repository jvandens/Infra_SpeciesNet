FROM pytorch/pytorch:2.1.2-cuda12.1-cudnn8-runtime

WORKDIR /opt

# System dependencies
RUN apt-get update && apt-get install -y \
    git \
    ffmpeg \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Install cameratrappAI
RUN pip install --no-cache-dir \
    git+https://github.com/google/cameratrapai.git

# Data mount
RUN mkdir -p /data

# Sanity check
RUN python - <<EOF
import torch
print("CUDA available:", torch.cuda.is_available())
print("CUDA version:", torch.version.cuda)
print("GPU:", torch.cuda.get_device_name(0))
EOF

ENTRYPOINT ["python"]
