FROM australiansynchrotron/epics:ubuntu16.04
COPY configure /tmp/configure
RUN apt-get update \
    && apt-get -y install curl re2c libhdf5-dev libx11-dev libxext-dev libxml2-dev \
                          libpng12-dev libbz2-dev libfreetype6-dev libtiff5-dev \
                          libgraphicsmagick++1-dev \
    && mkdir -p /opt/epics/base/extensions && cd /opt/epics/base/extensions \
    && curl -sLo msi.tgz http://www.aps.anl.gov/epics/download/extensions/msi1-7.tar.gz \
    && echo "040cb5d3fbab85925e5edc5b658792b16a17b4b825198fa1b95ac8eaa2a6cef6 msi.tgz" | sha256sum -c - \
    && tar -xf msi.tgz && rm msi.tgz && mv msi1-7 msi && cd msi \
    && make \
    && mkdir -p /opt/epics/modules/soft && cd /opt/epics/modules/soft \
    && curl -sLo synApps.tgz https://www1.aps.anl.gov/files/download/BCDA/synApps/tar/synApps_5_8.tar.gz \
    && echo "cd0f89d8d439ce6dafb7099b9fccd368edfcb3f84142ca04e19a8ec876012f6d synApps.tgz" | sha256sum -c - \
    && tar -xf synApps.tgz && rm synApps.tgz && mv synApps_5_8 synApps && cd synApps/support \
    && find . -name RELEASE \
              -exec sed -i -e 's|^EPICS_BASE=.*|EPICS_BASE=/opt/epics/base|' \
                           -e 's|^SUPPORT=.*|SUPPORT=/opt/epics/modules/soft/synApps/support|' \
                           {} \; \
    && sed -i -e 's|^QUADEM|#QUADEM|' configure/RELEASE \
    && mv /tmp/configure/areaDetector/CONFIG_SITE.local areaDetector-R2-0/configure/CONFIG_SITE.local \
    && rm areaDetector-R2-0/configure/CONFIG_SITE.local.linux-x86_64 \
    && make \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
