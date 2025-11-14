FROM nvcr.io/nvidia/ai-workbench/python-cuda122:1.0.3

# Trust additional network certificates
ARG CERT_URL=""
RUN if [ ! -z "$CERT_URL" ]; then curl "$CERT_URL" >> /etc/ssl/certs/ca-certificates.crt; fi

# Install torch and DGL with CUDA
RUN pip install --no-cache-dir \
    torch==2.2.2+cu121 --extra-index-url https://download.pytorch.org/whl/cu121 \
    dgl==1.1.3+cu121 --find-links https://data.dgl.ai/wheels/cu121/repo.html \
    git+https://github.com/NVIDIA/dllogger#egg=dllogger

# Install remaining dependencies
RUN pip install --no-cache-dir \
    e3nn==0.3.3 \
    wandb==0.12.0 \
    pynvml==11.0.0 \
    decorator==5.1.0 \
    hydra-core==1.3.2 \
    pyrsistent==0.19.3 \
    numpy==1.26.4 \
    pandas==2.3.1 \
    biopandas==0.5.1

# Copy RFdiffusion code into the container
# We use a fork of RFdiffusion that contains a few small additional features
# - Ability to run on CPU
# - Ability to run with less than 15 denoising iterations
RUN mkdir -p /opt/
RUN git clone --depth 1 https://github.com/prihoda/RFdiffusion-fork /opt/RFdiffusion

WORKDIR /

ENV DGLBACKEND="pytorch"
