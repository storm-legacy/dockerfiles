# packages:prod
ARG NODE_IMAGE=node:22
FROM ${NODE_IMAGE} AS base
WORKDIR /app

# packages:dev
FROM base AS dev
RUN npm i -D

# final:prod
FROM base AS final

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update -y \
  && apt-get install chromium -y \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

ARG USERNAME=app
ARG PUID=1000
ARG PGID=1000
RUN id "${USERNAME}" &> /dev/null && userdel -r app || true \
  && awk -F: '($3 == 1000) {print $1}' /etc/passwd | xargs -r userdel -r \
  && awk -F: '($3 == 1000) {print $1}' /etc/group | xargs -r groupdel \
  # Create user
  && groupadd -g ${PGID} ${USERNAME} \
  && useradd -m -s /bin/bash -u ${PUID} -g ${PGID} ${USERNAME}
