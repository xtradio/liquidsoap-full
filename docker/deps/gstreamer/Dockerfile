ARG BASE_IMAGE
FROM $BASE_IMAGE

ENTRYPOINT bash

MAINTAINER The Savonet Team <savonet-users@lists.sourceforge.net>

ARG OS
ARG DISTRIBUTION

ENV DEBFULLNAME="The Savonet Team"
ENV DEBEMAIL="savonet-users@lists.sourceforge.net"

RUN \
    sed -e 's#main#main contrib non-free#' -i /etc/apt/sources.list && \
    cat /etc/apt/sources.list | grep '^deb' | sed -e 's#deb #deb-src #g' >> /etc/apt/sources.list && \
    apt-get update && \
    apt-get install -y apt-transport-https ca-certificates devscripts && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 20D63CCDDD0F62C2 && \
    echo deb http://deb.liquidsoap.info/$OS $DISTRIBUTION main >> /etc/apt/sources.list && \
    apt-get update && \
    apt-get build-dep -y gstreamer1.0-libav && \
    apt-get -y autoclean && \
    apt-get -y clean

RUN \
    apt-get source gstreamer1.0-libav && \
    cd gst-libav1.0-* && \
    dch -i --distribution $DISTRIBUTION "Rebuild against updated ffmpeg" && \
    fakeroot debian/rules binary && \
    mkdir -p /tmp/debian/pkgs/${OS}_${DISTRIBUTION} && \
    cp ../*.deb /tmp/debian/pkgs/${OS}_${DISTRIBUTION} 
