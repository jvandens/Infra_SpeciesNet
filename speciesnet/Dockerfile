FROM pytorch/pytorch:2.1.2-cuda12.1-cudnn8-runtime

ENV TZ=Etc/UTC
ENV DEBIAN_FRONTEND=noninteractive

WORKDIR /opt

# ---- HARD STOP tzdata from prompting ----
RUN ln -fs /usr/share/zoneinfo/$TZ /etc/localtime && \
    echo $TZ > /etc/timezone
    
# System dependencies
RUN DEBIAN_FRONTEND=noninteractive \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        tzdata \
        git \
        ffmpeg \
        ca-certificates \
    && dpkg-reconfigure --frontend noninteractive tzdata \
    && rm -rf /var/lib/apt/lists/*

# Install cameratrappAI
RUN pip install --no-cache-dir \
    git+https://github.com/google/cameratrapai.git

# Data mount
RUN mkdir -p /data

# Sanity check
RUN python - <<EOF
import torch
print("Torch version:", torch.__version__)
print("CUDA build:", torch.version.cuda)
print("CUDA available (build-time):", torch.cuda.is_available())
EOF

ENTRYPOINT ["python"]
