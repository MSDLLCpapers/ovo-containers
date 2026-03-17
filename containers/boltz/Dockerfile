FROM nvcr.io/nvidia/ai-workbench/python-cuda122:1.0.3

# Trust additional network certificates
ARG CERT_URL=""
RUN if [ ! -z "$CERT_URL" ]; then curl "$CERT_URL" >> /etc/ssl/certs/ca-certificates.crt; fi

RUN pip install --no-cache-dir --upgrade pip setuptools wheel

RUN git clone https://github.com/jwohlwend/boltz.git \
    && cd boltz && git checkout cb04aeccdd480fd4db707f0bbafde538397fa2ac # NOTE: remember to update environment.yml too
RUN cd boltz; pip install --no-cache-dir -e .[cuda]
