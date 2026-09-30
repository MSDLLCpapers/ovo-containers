FROM python:3.12-alpine

# Trust additional network certificates
ARG CERT_URL=""
RUN if [ ! -z "$CERT_URL" ]; then wget -O - "$CERT_URL" >> /etc/ssl/certs/ca-certificates.crt; fi

RUN apk add --no-cache \
    build-base \
    bash

# Install pip dependencies
# NOTE: biotite 1.7.0 is not supported by hydride 1.2.3, this restriction can be removed later if support is fixed
RUN pip install --no-cache-dir biopython==1.85 biopandas scipy "biotite<1.7" pandas einops

ENTRYPOINT []
