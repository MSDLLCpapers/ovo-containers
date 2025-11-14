FROM python:3.12-alpine

# Trust additional network certificates
ARG CERT_URL=""
RUN if [ ! -z "$CERT_URL" ]; then wget -O - "$CERT_URL" >> /etc/ssl/certs/ca-certificates.crt; fi

RUN apk add --no-cache \
    build-base \
    bash \
    perl

# Install pip dependencies
RUN pip install --no-cache-dir biopython==1.85 pandas numpy

# Download ProteinSol
RUN wget -O /opt/proteinsol.zip https://protein-sol.manchester.ac.uk/cgi-bin/utilities/download_sequence_code.php
RUN unzip /opt/proteinsol.zip -d /opt

ENTRYPOINT []
