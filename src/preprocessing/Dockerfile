# Use the official Debian-hosted Python image
FROM python:3.8-slim-buster

# Tell pipenv where the shell is. 
# This allows us to use "pipenv shell" as a container entry point.
ENV PYENV_SHELL=/bin/bash

# JZ: update credential

# Can't use for vertex AI
# ENV GOOGLE_APPLICATION_CREDENTIALS=../../../secrets/ccb.json

# Ensure we have an up to date baseline, install dependencies 
RUN set -ex; \
    apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends build-essential && \
    pip install --no-cache-dir --upgrade pip && \
    pip install pipenv  && \
    pip install librosa && \
    pip install matplotlib && \
    apt-get --yes install libsndfile1 && \
    pip install --upgrade google-api-python-client && \
    pip install --upgrade google-cloud-storage && \
    apt-get --yes install vim && \
    mkdir -p /app

WORKDIR /app

# Add Pipfile, Pipfile.lock
ADD Pipfile Pipfile.lock /app/

RUN pipenv sync

# Source code
ADD . /app

# Entry point
ENTRYPOINT ["/bin/bash","./docker-entrypoint.sh"]
