FROM nvcr.io/nvidia/ai-workbench/python-cuda122:1.0.3

# Trust additional network certificates
ARG CERT_URL=""
RUN if [ ! -z "$CERT_URL" ]; then curl "$CERT_URL" >> /etc/ssl/certs/ca-certificates.crt; fi

RUN pip install --no-cache-dir \
    git+https://github.com/facebookresearch/esm.git@17a540a7efce4200849ca80863381b27c4b82011 \
    biopython \
    "torch<2.6" \
    torch_geometric \
    pandas \
    "numpy<2" \
    scipy \
    biotite==0.41.2

WORKDIR /