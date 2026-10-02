FROM nvidia/cuda:12.2.2-devel-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive

# Optionally rewrite the apt source lists at build time, e.g.
# --build-arg APT_SED='s|http://archive.ubuntu.com/ubuntu|https://<mirror-ubuntu>|g'
# You can chain multiple expressions with `;`
ARG APT_SED=""
RUN if [ -n "$APT_SED" ]; then \
        find /etc/apt/sources.list /etc/apt/sources.list.d/ -type f 2>/dev/null | xargs -r sed -i "$APT_SED" ; \
    fi

# Trust additional network certificates
ARG CERT_URL=""
RUN if [ -n "$CERT_URL" ]; then \
        apt-get update && apt-get install -y wget ca-certificates && \
        wget -O - "$CERT_URL" >> /etc/ssl/certs/ca-certificates.crt; \
    fi

# Install Python 3.12 and runtime deps. Note: python3-pip from Ubuntu 22.04
# is bound to the system Python 3.10 and can't run under 3.12 (no distutils);
# we bootstrap pip for 3.12 with ensurepip below instead.
RUN apt-get update && apt-get install -y --no-install-recommends \
       software-properties-common \
    && add-apt-repository ppa:deadsnakes/ppa \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
       python3.12 python3.12-venv python3.12-dev \
       git build-essential \
       libxrender1 libxext6 libexpat1 \
       libx11-6 libxcb1 libxau6 libxdmcp6 libfontconfig1 libfreetype6 \
    && rm -rf /var/lib/apt/lists/*

RUN python3.12 -m ensurepip --upgrade \
    && python3.12 -m pip install --no-cache-dir --upgrade pip setuptools wheel uv

# CUTLASS — required by DeepSpeed4Science EvoformerAttention JIT compile
RUN git clone --depth 1 https://github.com/NVIDIA/cutlass.git /opt/cutlass
ENV CUTLASS_PATH=/opt/cutlass

# DISCO source: Note that we're using a fork of the DISCO repo, because the original 
# requires downloading model files from HF at run time, which can now be overwritten 
# by environment variables and loaded from local model checkpoints: 
# DISCO_COMPONENTS_CIF, DISCO_COMPONENTS_PKL, and DISCO_DPLM_650M_PATH.
RUN git clone --depth 1 https://github.com/hugohrban/DISCO.git /opt/DISCO
WORKDIR /opt/DISCO

# Make uv trust the corporate CA bundle that we appended above
ENV UV_PYTHON_PREFERENCE=only-system
ENV UV_NATIVE_TLS=1
ENV SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
RUN uv sync

# Pin torch to a cu121 build so the image works on hosts with drivers that
# only support up to CUDA 12.2 (default wheels target cu126/cu128). Follows
# the documented procedure in DISCO's README.
RUN uv pip uninstall torch torchvision torchaudio || true
RUN uv pip install torch torchvision torchaudio --torch-backend=cu121

# CUDA toolkit from the base image — make sure CUDA_HOME is set so DeepSpeed
# can JIT-compile EvoformerAttention on first inference call.
ENV CUDA_HOME=/usr/local/cuda
ENV PATH=${CUDA_HOME}/bin:${PATH}
ENV LD_LIBRARY_PATH=${CUDA_HOME}/lib64:${LD_LIBRARY_PATH}

# Activate the venv by default
ENV PATH=/opt/DISCO/.venv/bin:${PATH}
ENV VIRTUAL_ENV=/opt/DISCO/.venv

# LigandMPNN install
ARG MODEL_PARAMS_VAR=/opt/LigandMPNN/model_params
RUN git clone --depth 1 https://github.com/dauparas/LigandMPNN.git /opt/LigandMPNN \
    && cp /opt/DISCO/packages/openfold/np/residue_constants.py /opt/LigandMPNN/openfold/np/residue_constants.py \
    && mkdir -p $MODEL_PARAMS_VAR

# LigandMPNN weights
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/ligandmpnn_v_32_005_25.pt -O $MODEL_PARAMS_VAR/ligandmpnn_v_32_005_25.pt
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/ligandmpnn_v_32_010_25.pt -O $MODEL_PARAMS_VAR/ligandmpnn_v_32_010_25.pt
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/ligandmpnn_v_32_020_25.pt -O $MODEL_PARAMS_VAR/ligandmpnn_v_32_020_25.pt
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/ligandmpnn_v_32_030_25.pt -O $MODEL_PARAMS_VAR/ligandmpnn_v_32_030_25.pt
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/global_label_membrane_mpnn_v_48_020.pt -O $MODEL_PARAMS_VAR/global_label_membrane_mpnn_v_48_020.pt
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/ligandmpnn_sc_v_32_002_16.pt -O $MODEL_PARAMS_VAR/ligandmpnn_sc_v_32_002_16.pt


ENTRYPOINT []
