FROM nvcr.io/nvidia/ai-workbench/python-cuda122:1.0.3

# Trust additional network certificates
ARG CERT_URL=""
RUN if [ ! -z "$CERT_URL" ]; then curl "$CERT_URL" >> /etc/ssl/certs/ca-certificates.crt; fi

RUN pip install --no-cache-dir \
    biopython \
    torch==2.4.1 \
    transformers==4.45.1

WORKDIR /
