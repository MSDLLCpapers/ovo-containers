FROM nvcr.io/nvidia/ai-workbench/python-cuda122:1.0.3

# Trust additional network certificates
ARG CERT_URL=""
RUN if [ ! -z "$CERT_URL" ]; then curl "$CERT_URL" >> /etc/ssl/certs/ca-certificates.crt; fi

RUN pip install --no-cache-dir --upgrade pip setuptools wheel

RUN git clone https://github.com/jwohlwend/boltz.git \
    && cd boltz && git checkout c9b6af1f13dd22d7ad394131928f0772ec37a82f # NOTE: remember to update environment.yml too
RUN cd boltz; pip install --no-cache-dir -e .[cuda]

RUN curl -OL https://sw-tools.rcsb.org/apps/MAXIT/maxit-v11.400-prod-src.tar.gz && \
  tar -xvzf maxit-v11.400-prod-src.tar.gz && \
  apt-get update && \
  apt-get --yes install build-essential bison flex && \
  cd maxit-v11.400-prod-src && \
  make binary

ENV RCSBROOT=/maxit-v11.400-prod-src
ENV PATH=${RCSBROOT}/bin:${PATH}
