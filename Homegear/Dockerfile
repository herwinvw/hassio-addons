# Taken from https://github.com/Homegear/Homegear-Docker/blob/master/rpi-stable/Dockerfile
#FROM resin/rpi-raspbian:stretch
#ARG BUILD_FROM=ghcr.io/hassio-addons/debian-base:7.5.0
ARG BUILD_FROM=ghcr.io/hassio-addons/debian-base/amd64:7.5.0


FROM $BUILD_FROM

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

MAINTAINER Jan Schulz-Hofen <hassio@0.jan.sh>
#ENV architecture=${BUILD_ARCH}

RUN apt-get update 
RUN apt-get -y install apt-transport-https wget ca-certificates apt-utils gnupg
RUN wget https://homegear.eu/packages/Release.key && apt-key add Release.key && rm Release.key
RUN echo 'deb https://homegear.eu/packages/Raspbian/ stretch/' >> /etc/apt/sources.list.d/homegear.list
#RUN apt-get update && apt-get -y install ntp tzdata homegear homegear-nodes-core homegear-nodes-extra homegear-homematicbidcos homegear-homematicwired homegear-insteon homegear-max homegear-philipshue homegear-sonos homegear-kodi homegear-ipcam homegear-beckhoff homegear-knx homegear-enocean homegear-intertechno homegear-influxdb vim less htop
RUN apt-get update && apt-get -y install ntp tzdata homegear homegear-nodes-core homegear-nodes-extra homegear-homematicbidcos homegear-max vim less htop
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
