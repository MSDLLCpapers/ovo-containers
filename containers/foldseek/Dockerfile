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

# Install Foldseek (AMD x86_64)
RUN wget https://github.com/steineggerlab/foldseek/releases/download/10-941cd33/foldseek-linux-avx2.tar.gz && \
    tar xvzf foldseek-linux-avx2.tar.gz && \
    rm foldseek-linux-avx2.tar.gz

# Also install Foldseek for ARM - Mac M1, will be in /arm/foldseek/bin/foldseek
RUN mkdir arm; cd arm; wget https://github.com/steineggerlab/foldseek/releases/download/10-941cd33/foldseek-linux-arm64.tar.gz && \
    tar xvzf foldseek-linux-arm64.tar.gz && \
    rm foldseek-linux-arm64.tar.gz

# Create wrapper foldseek script that chooses the correct binary
RUN echo 'FOLDSEEK=/foldseek/bin/foldseek; $FOLDSEEK -h 2>/dev/null >/dev/null || export FOLDSEEK=/arm/foldseek/bin/foldseek; $FOLDSEEK $@' \
    > /usr/bin/foldseek; chmod a+x /usr/bin/foldseek

# Install pip dependencies
RUN pip install --no-cache-dir biopython==1.85 pandas 

ENTRYPOINT []
