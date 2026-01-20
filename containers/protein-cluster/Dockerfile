FROM python:3.12-slim-bookworm

# Trust additional network certificates
ARG CERT_URL=""
RUN if [ ! -z "$CERT_URL" ]; then \
        apt-get update && apt-get install -y wget ca-certificates && \
        wget -O - "$CERT_URL" >> /etc/ssl/certs/ca-certificates.crt; \
    fi

# Install dependencies
RUN apt-get update && apt-get install -V -y \
    wget \
    build-essential \
    cmake 

RUN pip install --no-cache-dir \
    scipy \
    scikit-learn \
    pandas \
    numpy \
    umap-learn \
    seaborn \
    biopython==1.85

ENTRYPOINT []
