FROM docker

RUN set -xe \
 && apk add --no-cache \
      make \
      curl \
      py-pip \
 && pip3 install --upgrade pip \
 && apk add --no-cache --virtual .build-deps \
      gcc \
      rust \
      cargo \
      libffi-dev \
      musl-dev \
      openssl-dev \
      python3-dev \
 && pip3 install docker-compose \
 && apk del --no-network .build-deps \
 && rm -rf ~/.cache
