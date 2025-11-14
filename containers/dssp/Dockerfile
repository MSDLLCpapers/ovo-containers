FROM python:3.12-slim

# Trust additional network certificates
ARG CERT_URL=""
RUN if [ ! -z "$CERT_URL" ]; then \
        apt-get update && apt-get install -y wget ca-certificates && \
        wget -O - "$CERT_URL" >> /etc/ssl/certs/ca-certificates.crt; \
    fi

# Install dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    wget \
    build-essential \
    cmake \
    git \
    zlib1g-dev \
    libeigen3-dev \
    bash \
    && rm -rf /var/lib/apt/lists/*

# Install libcifpp from source
RUN git clone https://github.com/PDB-REDO/libcifpp.git \
    && cd libcifpp \
    && cmake -S . -B build \
    && cmake --build build \
    && cmake --install build \
    && ctest --test-dir build \
    && cd .. && rm -rf ./libcifpp && rm -rf ./libcifpp/build/_deps/my-eigen3-src/.git

# Install DSSP from source
RUN wget https://github.com/PDB-REDO/dssp/archive/refs/tags/v4.5.6.tar.gz \
    && tar -xvzf v4.5.6.tar.gz \
    && rm -f v4.5.6.tar.gz \
    && cd dssp-4.5.6 \
    && cmake -S . -B build \
    && cmake --build build \
    && cmake --install build \
    && cd .. && rm -r ./dssp-4.5.6 \
    && mkdssp -h

# Install pip dependencies
RUN pip install --no-cache-dir biopython==1.85 pandas MDAnalysis==2.7.0

ENTRYPOINT []