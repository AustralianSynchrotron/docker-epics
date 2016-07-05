FROM australiansynchrotron/epics:centos7
ENV USER=root \
    SUPPORT=/opt/epics/support \
    AREADETECTOR_VERSION=e23405877d56600a81f9c4711df71409dd2a0807 \
    ASYN_VERSION=R4-29 \
    CALC_VERSION=R3-6-1 \
    BUSY_VERSION=R1-6-1 \
    SSCAN_VERSION=R2-10-2 \
    AUTOSAVE_VERSION=R5-7-1
COPY configure /tmp/configure
WORKDIR /opt/epics/support
RUN yum -y install git libtiff-devel libjpeg-turbo-devel libxml2-devel \
    && curl -LO http://www.hdfgroup.org/ftp/HDF5/current/bin/linux-centos7-x86_64-gcc485/hdf5-1.8.17-linux-centos7-x86_64-gcc485-shared.tar.gz \
    && tar -xf hdf5* && rm -f hdf5*.tar.gz \
    && mv -f hdf5* /usr/local/hdf5 \
    && git clone -b $ASYN_VERSION --single-branch https://github.com/epics-modules/asyn \
    && git clone -b $CALC_VERSION --single-branch https://github.com/epics-modules/calc \
    && git clone -b $BUSY_VERSION --single-branch https://github.com/epics-modules/busy \
    && git clone -b $SSCAN_VERSION --single-branch https://github.com/epics-modules/sscan \
    && git clone -b $AUTOSAVE_VERSION --single-branch https://github.com/epics-modules/autosave \
    && git clone https://github.com/areaDetector/areaDetector \
    && cd areaDetector \
    && git checkout $AREADETECTOR_VERSION \
    && git submodule init ADCore \
    && git submodule init ADExample \
    && git submodule update \
    && cd .. \
    && find . -name .git -exec rm -rf {} \+ \
    && mv -f /tmp/configure/asyn/* asyn/configure/ \
    && mv -f /tmp/configure/calc/* calc/configure/ \
    && mv -f /tmp/configure/busy/* busy/configure/ \
    && mv -f /tmp/configure/sscan/* sscan/configure/ \
    && mv -f /tmp/configure/autosave/* autosave/configure/ \
    && mv -f /tmp/configure/areaDetector/* areaDetector/configure/ \
    && cd asyn && make && cd .. \
    && cd calc && make && cd .. \
    && cd busy && make && cd .. \
    && cd sscan && make && cd .. \
    && cd autosave && make && cd .. \
    && cd areaDetector && make
