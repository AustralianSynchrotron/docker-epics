FROM ubuntu:16.04
RUN apt-get update \
    && apt-get -y install curl g++ make libperl-dev libreadline-dev \
    && mkdir /opt/epics \
    && cd /opt/epics \
    && curl -o base.tgz http://www.aps.anl.gov/epics/download/base/baseR3.14.12.5.tar.gz \
    && echo "ef05593edb70c87d6ae02c3e1e7c4f3f325934f1b98deaba49203d9343861d72 base.tgz" | sha256sum -c - \
    && tar -xf base.tgz \
    && mv base-3.14.12.5 base \
    && cd base \
    && make install \
    && rm /opt/epics/base.tgz \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
ENV PATH=/opt/epics/base/bin/linux-x86_64:$PATH \
    EPICS_BASE=/opt/epics/base \
    EPICS_HOST_ARCH=linux-x86_64
