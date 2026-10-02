FROM python:3.11-slim-bookworm

# Trust additional network certificates
ARG CERT_URL=""
RUN if [ ! -z "$CERT_URL" ]; then \
        apt-get update && apt-get install -y --no-install-recommends wget ca-certificates && \
        rm -rf /var/lib/apt/lists/* && \
        wget -O - "$CERT_URL" >> /etc/ssl/certs/ca-certificates.crt; \
    fi
ENV SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt

# Install system dependencies - procps required by nextflow,
# build-essential required to build ProDy from source
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential git jq wget procps \
    && rm -rf /var/lib/apt/lists/*

# LigandMPNN fork: pip installable, provides the `ligandmpnn` command
# and bundles all model weights (ProteinMPNN, LigandMPNN, SolubleMPNN,
# membrane, side-chain packing, PottsMPNN, CyclicMPNN, HyperMPNN)
ARG LIGANDMPNN_COMMIT=3280ca6f08f7363329d734e21b7ca4d710c39b16
RUN pip install --no-cache-dir \
    setuptools==81.0.0 \
    git+https://github.com/prihoda/LigandMPNN-fork.git@${LIGANDMPNN_COMMIT}

WORKDIR /
