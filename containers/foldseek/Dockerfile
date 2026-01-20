FROM python:3.12-slim

# Trust additional network certificates
ARG CERT_URL=""
RUN if [ ! -z "$CERT_URL" ]; then \
        apt-get update && apt-get install -y wget ca-certificates && \
        wget -O - "$CERT_URL" >> /etc/ssl/certs/ca-certificates.crt; \
    fi

# Install dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    procps \
    wget \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Install Foldseek
RUN wget https://github.com/steineggerlab/foldseek/releases/download/10-941cd33/foldseek-linux-arm64.tar.gz && \
    tar xvzf foldseek-linux-arm64.tar.gz && \
    rm foldseek-linux-arm64.tar.gz

# Add Foldseek to PATH
ENV PATH=/foldseek/bin/:$PATH

# Install pip dependencies
RUN pip install --no-cache-dir biopython==1.85 pandas 

ENTRYPOINT []