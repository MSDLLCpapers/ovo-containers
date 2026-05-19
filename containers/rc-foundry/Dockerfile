FROM python:3.12-slim-bookworm

# Trust additional network certificates
ARG CERT_URL=""
RUN if [ ! -z "$CERT_URL" ]; then \
        apt-get update && apt-get install -y --no-install-recommends wget ca-certificates && \
        rm -rf /var/lib/apt/lists/* && \
        wget -O - "$CERT_URL" >> /etc/ssl/certs/ca-certificates.crt; \
    fi

RUN apt-get update && apt-get install -y --no-install-recommends \
    procps \
    && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir gemmi "rc-foundry[all]"

ENTRYPOINT []
