ARG BUILD_FROM=ghcr.io/home-assistant/base-debian:trixie
FROM $BUILD_FROM

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install deCONZ dependencies
RUN \
    set -x \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        curl \
        openbox \
        iproute2 \
        iputils-ping \
        kmod \
        libcap2-bin \
        libqt6gui6 \
        libqt6network6 \
        libqt6serialport6 \
        libqt6sql6 \
        libqt6websockets6 \
        libqt6widgets6 \
        libqt6qml6 \
        libqt6core5compat6 \
        lsof \
        netcat-traditional \
        nginx \
        novnc \
        sqlite3 \
        tigervnc-common \
        tigervnc-standalone-server \
        udev \
        wget \
        xfonts-base \
        xfonts-scalable \
    && rm -rf /var/lib/apt/lists/*

# Install deCONZ
ARG DECONZ_VERSION=2.33.2
ARG TARGETARCH
RUN \
    set -x \
    && curl -q -L -o /deconz.deb \
        "https://github.com/dresden-elektronik/deconz-rest-plugin/releases/download/v${DECONZ_VERSION}/deconz-qt6_${DECONZ_VERSION}_${TARGETARCH}.deb" \
    && dpkg -i /deconz.deb \
    && rm -f /deconz.deb \
    && chown root:root /usr/bin/deCONZ* \
    && sed -i 's/\/root/\/data/' /etc/passwd

COPY rootfs /

ENV S6_STAGE2_HOOK=/etc/s6-overlay/scripts/enable-check.sh

LABEL \
    org.opencontainers.image.title="Home Assistant App: deCONZ" \
    org.opencontainers.image.description="Control a Zigbee network with ConBee or RaspBee by dresden elektronik" \
    org.opencontainers.image.source="https://github.com/home-assistant/app-deconz" \
    org.opencontainers.image.licenses="Apache License 2.0"
