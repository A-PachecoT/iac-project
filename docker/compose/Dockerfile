FROM ubuntu:latest

RUN apt-get update && apt-get install -y \
    git \
    curl \
    sudo

# Crear usuario bender
RUN useradd -m -s /bin/bash bender && \
    usermod -aG sudo bender

WORKDIR /home/bender

# Cambiar al usuario bender
USER bender
