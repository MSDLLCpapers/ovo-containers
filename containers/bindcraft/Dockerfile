FROM nvcr.io/nvidia/ai-workbench/python-cuda122:1.0.3

RUN ln -s /usr/bin/python3 /usr/bin/python

# Trust additional network certificates
ARG CERT_URL=""
RUN if [ ! -z "$CERT_URL" ]; then curl "$CERT_URL" >> /etc/ssl/certs/ca-certificates.crt; fi
ENV SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt

# Dependencies
RUN apt-get update \
    && apt-get install -y bzip2 libgfortran5 ffmpeg \
	&& apt-get clean && rm -rf /var/cache/apt/lists /var/lib/apt/lists

# Clone and install ColabDesign (AlphaFold2, ProteinMPNN)
RUN git clone --depth 1 https://github.com/sokrypton/ColabDesign.git /opt/colabdesign \
	&& cd /opt/colabdesign \
	&& pip install --no-cache-dir . jax[cuda12]==0.5.1 matplotlib==3.7.1 numpy"<2.0.0" seaborn tqdm

# Install PyRosetta
RUN cd /tmp; pip install --no-cache-dir pyrosetta-installer \
    && python -c 'import pyrosetta_installer; pyrosetta_installer.install_pyrosetta()'

# Clone BindCraft repository
RUN mkdir -p /content/ \
  && git clone --depth 1 https://github.com/martinpacesa/BindCraft /content/bindcraft/ \
  && chmod +x /content/bindcraft/functions/dssp \
  && chmod +x /content/bindcraft/functions/DAlphaBall.gcc

WORKDIR /
