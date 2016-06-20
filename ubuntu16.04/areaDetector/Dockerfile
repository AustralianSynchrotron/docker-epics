FROM australiansynchrotron/epics:ubuntu16.04-synapps5.8
COPY configure /tmp/configure
RUN apt-get update && apt-get -y install git \
    && cd /opt/epics/modules/soft/synApps/support \
    && make clean \
    && rm -r areaDetector-* \
    && git clone --recursive -b R2-4 --single-branch \
                 https://github.com/areaDetector/areaDetector.git \
    && rm -rf areaDetector/.git \
    && mv /tmp/configure/areaDetector/* areaDetector/configure/ \
    && find . -name RELEASE \
              -exec sed -E -i \
                        -e 's|areaDetector-R2-0|areaDetector|' \
                        -e 's|ADCore-R2-2|ADCore|' \
                        -e 's|ADBinaries-R2-2|ADBinaries|' \
                        {} \; \
    && make \
    && rm -rf /tmp/configure \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
ENV EPICS_CA_MAX_ARRAY_BYTES=96000000
