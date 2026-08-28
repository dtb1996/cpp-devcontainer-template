FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

ARG USERNAME=dillon
ARG USER_UID=1000
ARG USER_GID=1000

RUN apt-get update && apt-get install -y \
    build-essential \
    clang \
    clangd \
    clang-format \
    clang-tidy \
    cmake \
    ninja-build \
    gdb \
    lldb \
    git \
    pkg-config \
    python3 \
    python3-pip \
    sudo \
    && rm -rf /var/lib/apt/lists/*

# Ubuntu 24.04 already provides a user/group with UID/GID 1000.
# Rename that user and group to match the development user.
RUN usermod --login ${USERNAME} \
            --home /home/${USERNAME} \
            --move-home ubuntu \
    && groupmod --new-name ${USERNAME} ubuntu \
    && echo "${USERNAME} ALL=(root) NOPASSWD:ALL" \
       > /etc/sudoers.d/${USERNAME} \
    && chmod 0440 /etc/sudoers.d/${USERNAME}

USER ${USERNAME}
WORKDIR /home/${USERNAME}
