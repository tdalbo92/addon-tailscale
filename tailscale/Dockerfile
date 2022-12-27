ARG BUILD_FROM=ghcr.io/hassio-addons/base/aarch64:12.0.0
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base
ARG BUILD_ARCH=aarch64
RUN apk update
RUN \
    apk add --no-cache \
        ip6tables=1.8.8-r1 \
        ipcalc=1.0.1-r0 \
        iproute2=5.17.0-r0 \
        iptables=1.8.8-r1 \
        nginx=1.22.1-r0 \
    \
    && ln -sf /sbin/xtables-nft-multi /sbin/ip6tables \
    && ln -sf /sbin/xtables-nft-multi /sbin/iptables \
    \
    && if [ "${BUILD_ARCH}" = "aarch64" ]; then ARCH="arm64"; fi \
    && if [ "${BUILD_ARCH}" = "armhf" ]; then ARCH="arm"; fi \
    && if [ "${BUILD_ARCH}" = "armv7" ]; then ARCH="arm"; fi \
    && if [ "${BUILD_ARCH}" = "amd64" ]; then ARCH="amd64"; fi \
    && if [ "${BUILD_ARCH}" = "i386" ]; then ARCH="386"; fi \
    \
    && curl -L -s \
        "https://pkgs.tailscale.com/stable/tailscale_1.34.1_${ARCH}.tgz" \
        | tar zxvf - -C /opt/ --strip-components 1 \
    \
    && rm -f -r \
        /opt/systemd

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH=aarch64
ARG BUILD_DATE=2022-12-01T13:35:00Z
ARG BUILD_DESCRIPTION=Zero config VPN for building secure networks
ARG BUILD_NAME=Tailscale
ARG BUILD_REF=331d553a5eb90401c071021bae5dd24ce3993500
ARG BUILD_REPOSITORY=tdalbo92/addon-tailscale
ARG BUILD_VERSION=331d553

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Tom Dalbo <code@dalbo.net>" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Home Assistant Community Add-ons" \
    org.opencontainers.image.authors="Tom Dalbo <code@dalbo.net>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}
