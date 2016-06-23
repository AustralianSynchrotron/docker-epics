FROM australiansynchrotron/epics:ubuntu16.04-synapps5.8
COPY configure /tmp/configure
ENV FFMPEG_VERSION=2.8.7 \
    YASM_VERSION=1.3.0 \
    NULLHTTPD_VERSION=0-5-1dls5
RUN apt-get update && apt-get -y install git \
    && cd /opt/epics/modules/soft/synApps/support \
    && make clean \
    && rm -r areaDetector-* \
    && git clone --recursive -b R2-4 --single-branch \
                 https://github.com/areaDetector/areaDetector.git \
    && rm -rf areaDetector/.git \
    && find . -name RELEASE \
              -exec sed -E -i \
                        -e 's|areaDetector-R2-0|areaDetector|' \
                        -e 's|ADCore-R2-2|ADCore|' \
                        -e 's|ADBinaries-R2-2|ADBinaries|' \
                        {} \; \
    && find . -name RELEASE -exec sed -i -e 's|ADEXAMPLE_TOP|ADEXAMPLE|' {} \; \
    && curl -sLO http://controls.diamond.ac.uk/downloads/support/nullhttpd/${NULLHTTPD_VERSION}/nullhttpd-${NULLHTTPD_VERSION}.tgz \
    && echo "2a377785b5cb2d1c36a1413bfcb939cdb587d83d65e250d6d3a16ea4b3767665 nullhttpd-${NULLHTTPD_VERSION}.tgz" | sha256sum -c - \
    && tar -xf nullhttpd-${NULLHTTPD_VERSION}.tgz && rm nullhttpd-${NULLHTTPD_VERSION}.tgz \
    && mv nullhttpd-${NULLHTTPD_VERSION} nullhttpd \
    && cd nullhttpd \
    && sed -i -e '/#include <string.h>/d' src/nullhttpd.h \
    && sed -i -e 's|^EPICS_BASE=.*|EPICS_BASE=/opt/epics/base|' \
              -e 's|^TEMPLATE_TOP|#TEMPLATE_TOP|' \
              configure/RELEASE \
    && make \
    && cd /opt/epics/modules/soft/synApps/support/areaDetector/ffmpegServer \
    && find . -name RELEASE -exec sed -i -e 's|FFMPEGSERVER_TOP|FFMPEGSERVER|' {} \; \
    && cd vendor/ \
    && curl -sLO http://www.tortall.net/projects/yasm/releases/yasm-${YASM_VERSION}.tar.gz \
    && echo "3dce6601b495f5b3d45b59f7d2492a340ee7e84b5beca17e48f862502bd5603f yasm-${YASM_VERSION}.tar.gz" | sha256sum -c - \
    && tar -xf yasm-${YASM_VERSION}.tar.gz && rm yasm-${YASM_VERSION}.tar.gz \
    && mv yasm-${YASM_VERSION} yasm \
    && curl -sLO https://ffmpeg.org/releases/ffmpeg-${FFMPEG_VERSION}.tar.gz \
    && echo "cb06cef3f89e7c7c994dc15d6be3da68ce26e2646b2156a830b00981fcd9c924 ffmpeg-${FFMPEG_VERSION}.tar.gz" | sha256sum -c - \
    && tar -xf ffmpeg-${FFMPEG_VERSION}.tar.gz && rm ffmpeg-${FFMPEG_VERSION}.tar.gz  \
    && mv ffmpeg-${FFMPEG_VERSION} ffmpeg \
    && cd /opt/epics/modules/soft/synApps/support \
    && mv /tmp/configure/areaDetector/* areaDetector/configure/ \
    && make \
    && rm -rf /tmp/configure \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
