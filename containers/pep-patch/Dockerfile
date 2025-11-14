FROM python:3.12-slim

# Install dependencies
RUN apt-get update && apt-get install -y \
    git \
    wget \
    unzip \
    procps \
    && rm -rf /var/lib/apt/lists/*

# Trust additional network certificates
ARG CERT_URL=""
RUN if [ ! -z "$CERT_URL" ]; then wget -O - "$CERT_URL" >> /etc/ssl/certs/ca-certificates.crt; fi

RUN pip install --no-cache-dir \
    git+https://github.com/liedllab/surface_analyses.git@65da35b964be296b7b1b566c08546ec588349a4f

# Download APBS and extract it to the desired location
RUN wget https://github.com/Electrostatics/apbs/releases/download/v3.4.1/APBS-3.4.1.Linux.zip -O /tmp/APBS-3.4.1.Linux.zip \
    && unzip /tmp/APBS-3.4.1.Linux.zip -d /opt \
    && rm /tmp/APBS-3.4.1.Linux.zip

ENV PATH="/opt/APBS-3.4.1.Linux/bin:${PATH}"

COPY resources /opt/resources

ENTRYPOINT []
