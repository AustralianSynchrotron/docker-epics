FROM australiansynchrotron/epics:centos7-areadetector2.4
ENV FFMPEGSERVER_VERSION=4086745dc26d50ce7f57e2b3db931edefa0626e4 \
    NULLHTTPD_VERSION=0-5-1dls5 \
    YASM_VERSION=1.3.0 \
    FFMPEG_VERSION=2.8.7
COPY configure /tmp/configure
RUN cd "$SUPPORT" \
    && curl -LO http://controls.diamond.ac.uk/downloads/support/nullhttpd/${NULLHTTPD_VERSION}/nullhttpd-${NULLHTTPD_VERSION}.tgz \
    && tar -xf nullhttpd-${NULLHTTPD_VERSION}.tgz && rm -f nullhttpd-${NULLHTTPD_VERSION}.tgz \
    && mv nullhttpd-${NULLHTTPD_VERSION} nullhttpd \
    && cd nullhttpd \
    && sed -i -e '/#include <string.h>/d' src/nullhttpd.h \
    && mv -f /tmp/configure/nullhttpd/* configure/ \
    && make \
    && cd "${SUPPORT}/areaDetector" \
    && git clone https://github.com/areaDetector/ffmpegServer \
    && cd ffmpegServer \
    && git checkout "$FFMPEGSERVER_VERSION" \
    && rm -rf .git \
    && cd vendor \
    && curl -LO http://www.tortall.net/projects/yasm/releases/yasm-${YASM_VERSION}.tar.gz \
    && tar -xf yasm-${YASM_VERSION}.tar.gz && rm -f yasm-${YASM_VERSION}.tar.gz \
    && mv yasm-${YASM_VERSION} yasm \
    && curl -LO https://ffmpeg.org/releases/ffmpeg-${FFMPEG_VERSION}.tar.gz \
    && tar -xf ffmpeg-${FFMPEG_VERSION}.tar.gz && rm -f ffmpeg-${FFMPEG_VERSION}.tar.gz \
    && mv ffmpeg-${FFMPEG_VERSION} ffmpeg \
    && cd .. \
    && make
