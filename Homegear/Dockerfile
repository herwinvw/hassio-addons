# Taken from https://github.com/Homegear/Homegear-Docker/blob/master/rpi-stable/Dockerfile
#FROM resin/rpi-raspbian:stretch
#ARG BUILD_FROM=ghcr.io/hassio-addons/debian-base:7.5.0
ARG BUILD_FROM=ghcr.io/hassio-addons/debian-base/amd64:7.6.0
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base system
ARG BUILD_ARCH=amd64
RUN \
    ARCH="${BUILD_ARCH}" \
    && if [ "${BUILD_ARCH}" = "aarch64" ]; then ARCH="arm64"; fi \
    && if [ "${BUILD_ARCH}" = "armv7" ]; then ARCH="armhf"; fi \
    && apt-get update 
RUN apt-get -y install apt-transport-https wget ca-certificates apt-utils gnupg
#RUN wget https://homegear.eu/packages/Release.key && apt-key add Release.key && rm Release.key
#RUN echo 'deb https://homegear.eu/packages/Debian/ wheezy/' >> /etc/apt/sources.list.d/homegear.list
RUN curl -fsSL https://apt.homegear.eu/Release.key | gpg --dearmor -o /usr/share/keyrings/homegear-archive-keyring.gpg
RUN echo 'deb [signed-by=/usr/share/keyrings/homegear-archive-keyring.gpg] https://apt.homegear.eu/debian/bookworm/homegear/stable/ bookworm main' > /etc/apt/sources.list.d/homegear.list 
#RUN apt-get update && apt-get -y install ntp tzdata homegear homegear-nodes-core homegear-nodes-extra homegear-homematicbidcos homegear-homematicwired homegear-insteon homegear-max homegear-philipshue homegear-sonos homegear-kodi homegear-ipcam homegear-beckhoff homegear-knx homegear-enocean homegear-intertechno homegear-influxdb vim less htop
RUN apt-get -y install ntp tzdata vim less htop
RUN apt-get update && apt-get -y install homegear homegear-nodes-core homegear-nodes-extra homegear-homematicbidcos homegear-max
RUN rm -f /etc/homegear/dh1024.pem
RUN rm -f /etc/homegear/homegear.crt
RUN rm -f /etc/homegear/homegear.key
RUN cp -R /etc/homegear /etc/homegear.config
RUN cp -R /var/lib/homegear /var/lib/homegear.data

# Inspired by Dockerfile from visevision
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY start.sh /start.sh
RUN chmod +x /start.sh
CMD /start.sh

EXPOSE 2001 2002 2003
