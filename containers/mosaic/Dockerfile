FROM python:3.12-slim-bookworm

# Trust additional network certificates
ARG CERT_URL=""

# Install system dependencies
RUN apt-get update \
    && apt-get install -y --no-install-recommends git ca-certificates wget \
    && rm -rf /var/lib/apt/lists/* \
    && if [ -n "$CERT_URL" ]; then \
        wget -O - "$CERT_URL" >> /etc/ssl/certs/ca-certificates.crt; \
    fi

ENV SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt

# Install uv
COPY --from=ghcr.io/astral-sh/uv:0.7 /uv /uvx /usr/local/bin/

# Clone and install mosaic with uv (handles all git-sourced deps)
RUN git clone https://github.com/escalante-bio/mosaic.git /opt/mosaic

WORKDIR /opt/mosaic

RUN uv sync --group jax-cuda \
    && uv cache clean

ENV PATH="/opt/mosaic/.venv/bin:$PATH"

WORKDIR /
