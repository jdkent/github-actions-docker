FROM python:3.7-slim as builder
LABEL org.opencontainers.image.source=https://github.com/jdkent/github-actions-docker
ARG DEBIAN_FRONTEND="noninteractive"
WORKDIR /opt/pliers
COPY . .
# Install dependencies into a virtual environment so they can be easily copied into
# the second stage.
RUN python -m venv /opt/venv
RUN python -m pip install pip-install-test

FROM python:3.7-slim as base
LABEL org.opencontainers.image.source=https://github.com/jdkent/github-actions-docker
# Empty top level directories to facilitate use of the image in singularity
# on a box with kernel lacking overlay FS support
RUN mkdir -p /data /backup

FROM base as executable
LABEL org.opencontainers.image.source=https://github.com/jdkent/github-actions-docker
RUN useradd --create-home --shell /bin/bash pliers
COPY --from=builder --chown=pliers /opt/venv /opt/venv
COPY --from=builder --chown=pliers /opt/pliers /opt/pliers
ENV PATH="/opt/venv/bin:$PATH"
USER pliers
WORKDIR /work

FROM executable as docs
LABEL org.opencontainers.image.source=https://github.com/jdkent/github-actions-docker
CMD echo "yes"