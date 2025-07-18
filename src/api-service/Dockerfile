# Use the official Debian-hosted Python image
FROM python:3.9-slim-buster

RUN echo "Begin api-service dockerfile"

ARG DEBIAN_PACKAGES="build-essential git"

# Prevent apt from showing prompts
ENV DEBIAN_FRONTEND=noninteractive

# Python wants UTF-8 locale
ENV LANG=C.UTF-8

# Tell pipenv where the shell is. This allows us to use "pipenv shell" as a
# container entry point.
ENV PYENV_SHELL=/bin/bash

# Tell Python to disable buffering so we don't lose any logs.
ENV PYTHONUNBUFFERED=1

# Ensure we have an up to date baseline, install dependencies and
# create a user so we don't run the app as root
RUN set -ex; \
    for i in $(seq 1 8); do mkdir -p "/usr/share/man/man${i}"; done && \
    apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends $DEBIAN_PACKAGES && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    pip install --no-cache-dir --upgrade pip && \
    pip install pipenv && \
    useradd -ms /bin/bash app -d /home/app -u 1000 -p "$(openssl passwd -1 Passw0rd)" && \
    mkdir -p /app && \
    mkdir -p /persistent && \
    chown app:app /persistent && \
    chown app:app /app

# Expose port of API service
EXPOSE 9000

# Switch to the new user
# TODO - this is not working. Need to figure out why.
# USER app
WORKDIR /app

# Install python packages
# Comment out if updating pipfile.lock (?)
ADD --chown=app:app Pipfile Pipfile.lock /app/

# # Uncomment only if generating new lock file.
# USER root
# RUN pipenv lock

RUN pipenv sync

# Add the rest of the source code. This is done last so we don't invalidate all
# layers when we change a line of code.
ADD --chown=app:app . /app

# Entry point
ENTRYPOINT ["/bin/bash","./docker-custom-entrypoint.sh"]

# # Not needed since included in docker-entrypoint.sh
# CMD ["uvicorn", "api-service:app", "--host", "0.0.0.0", "--port", "9000"]