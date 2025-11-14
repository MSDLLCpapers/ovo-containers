FROM nvcr.io/nvidia/ai-workbench/python-cuda122:1.0.3

# Trust additional network certificates
ARG CERT_URL=""
RUN if [ ! -z "$CERT_URL" ]; then curl "$CERT_URL" >> /etc/ssl/certs/ca-certificates.crt; fi
ENV SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt

RUN ln -s /usr/bin/python3 /usr/bin/python

# Install Python, pip, and build-essential
RUN apt-get update \
    && apt-get install -y wget \
    build-essential \
    jq

# Pip dependencies
RUN pip install \
    --no-cache-dir \
    numpy==1.23.5 \
    scipy==1.12.0 \
    biopython==1.79 \
    ml-collections \
    dm-tree \
    ProDy==2.4.1 \
    torch==2.2.1

ARG MODEL_PARAMS_VAR=/opt/LigandMPNN/model_params

# TODO: Put in a single run
RUN mkdir -p /opt/ \
    && git clone --depth 1 https://github.com/dauparas/LigandMPNN.git /opt/LigandMPNN \
    && wget -O /opt/LigandMPNN/openfold/np/residue_constants.py https://github.com/aqlaboratory/openfold/raw/fbfbd8080f1d6abae63b5907c7ad6e546cc1ef23/openfold/np/residue_constants.py \
    && mkdir -p $MODEL_PARAMS_VAR

# Original ProteinMPNN weights
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/proteinmpnn_v_48_002.pt -O $MODEL_PARAMS_VAR/proteinmpnn_v_48_002.pt
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/proteinmpnn_v_48_010.pt -O $MODEL_PARAMS_VAR/proteinmpnn_v_48_010.pt
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/proteinmpnn_v_48_020.pt -O $MODEL_PARAMS_VAR/proteinmpnn_v_48_020.pt
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/proteinmpnn_v_48_030.pt -O $MODEL_PARAMS_VAR/proteinmpnn_v_48_030.pt

# LigandMPNN weights
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/ligandmpnn_v_32_005_25.pt -O $MODEL_PARAMS_VAR/ligandmpnn_v_32_005_25.pt
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/ligandmpnn_v_32_010_25.pt -O $MODEL_PARAMS_VAR/ligandmpnn_v_32_010_25.pt
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/ligandmpnn_v_32_020_25.pt -O $MODEL_PARAMS_VAR/ligandmpnn_v_32_020_25.pt
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/ligandmpnn_v_32_030_25.pt -O $MODEL_PARAMS_VAR/ligandmpnn_v_32_030_25.pt

# SolubleMPNN weights
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/solublempnn_v_48_002.pt -O $MODEL_PARAMS_VAR/solublempnn_v_48_002.pt
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/solublempnn_v_48_010.pt -O $MODEL_PARAMS_VAR/solublempnn_v_48_010.pt
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/solublempnn_v_48_020.pt -O $MODEL_PARAMS_VAR/solublempnn_v_48_020.pt
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/solublempnn_v_48_030.pt -O $MODEL_PARAMS_VAR/solublempnn_v_48_030.pt

# ProteinMPNN with global membrane label weights
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/global_label_membrane_mpnn_v_48_020.pt -O $MODEL_PARAMS_VAR/global_label_membrane_mpnn_v_48_020.pt

# ProteinMPNN with per residue membrane label weights
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/per_residue_label_membrane_mpnn_v_48_020.pt -O $MODEL_PARAMS_VAR/per_residue_label_membrane_mpnn_v_48_020.pt

# Side-chain package weights
RUN wget -q https://files.ipd.uw.edu/pub/ligandmpnn/ligandmpnn_sc_v_32_002_16.pt -O $MODEL_PARAMS_VAR/ligandmpnn_sc_v_32_002_16.pt

ENV PYTHONPATH=/opt/LigandMPNN

WORKDIR /
