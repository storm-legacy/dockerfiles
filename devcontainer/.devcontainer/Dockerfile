FROM ubuntu:24.04 AS base

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    ca-certificates build-essential \
    git curl wget gnupg2 openssl openssh-client \
    nano vim \
    sudo \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

ARG USERNAME=vscode
ARG PUID=1000
ARG PGID=$PUID
RUN id "${USERNAME}" &> /dev/null && userdel -r app || true \
    && awk -F: '($3 == 1000) {print $1}' /etc/passwd | xargs -r userdel -r \
    && awk -F: '($3 == 1000) {print $1}' /etc/group | xargs -r groupdel \
    # Create user
    && groupadd -g ${PGID} ${USERNAME} \
    && useradd -m -s /bin/bash -u ${PUID} -g ${PGID} ${USERNAME}

USER ${USERNAME}
WORKDIR /workspace

CMD [ "bash" ]

