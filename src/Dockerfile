# syntax=docker/dockerfile:1
FROM debian:trixie-slim

ARG deb_filename="aptly_amd64.deb"

ENV USER_NAME="aptly-api"
ENV DEFAULT_CONFIG_PATH="/etc/aptly.conf"
ENV DEBIAN_FRONTEND="noninteractive"
COPY "./docker-entrypoint" "/usr/local/libexec/"
WORKDIR "/var/lib/aptly-api"

RUN --mount="type=bind,source=./${deb_filename},target=/tmp/${deb_filename}" <<_EOF_
apt-get update
apt-get install \
    --assume-yes \
    --no-install-recommends \
    "/tmp/${deb_filename}" \
    "gpg-agent"
_EOF_

RUN <<_EOF_
groupadd --gid 1000 "${USER_NAME}"
useradd --create-home --uid 1000 --gid 1000 "${USER_NAME}"
chown "${USER_NAME}:${USER_NAME}" "/var/lib/aptly-api"
_EOF_

COPY \
    --chown="root:${USER_NAME}" \
    --chmod=640 \
    "./aptly.conf" \
    "${DEFAULT_CONFIG_PATH}"

USER "${USER_NAME}"
VOLUME ["/var/lib/aptly-api"]
ENTRYPOINT ["/usr/local/libexec/docker-entrypoint"]
