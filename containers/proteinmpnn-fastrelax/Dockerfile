FROM nvcr.io/nvidia/ai-workbench/python-cuda122:1.0.3

RUN ln -s /usr/bin/python3 /usr/bin/python

# Trust additional network certificates
ARG CERT_URL=""
RUN if [ ! -z "$CERT_URL" ]; then curl "$CERT_URL" >> /etc/ssl/certs/ca-certificates.crt; fi
ENV SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt

# Dependencies
RUN apt-get update \
    && apt-get install -y bzip2 \
	&& apt-get clean && rm -rf /var/cache/apt/lists /var/lib/apt/lists

# Install PyRosetta
RUN cd /tmp; pip install --no-cache-dir pyrosetta-installer \
    && python -c 'import pyrosetta_installer; pyrosetta_installer.install_pyrosetta()'

# Pip dependencies
RUN pip install \
    --no-cache-dir \
    --extra-index-url https://download.pytorch.org/whl/cpu \
    torch==2.2.2 \
    numpy==1.26.4 \
    biopython \
    ml-collections \
    openmm \
    scipy==1.13.0 \
    git+https://github.com/openmm/pdbfixer

# Copy lib submodules
RUN mkdir -p /opt/
RUN git clone --depth 1 https://github.com/nrbennet/dl_binder_design.git /opt/dl_binder_design
RUN git clone --depth 1 https://github.com/dauparas/ProteinMPNN.git /opt/ProteinMPNN

WORKDIR /
