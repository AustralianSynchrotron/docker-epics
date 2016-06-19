FROM centos:centos6
RUN yum -y install gcc-c++ perl-devel readline-devel perl-Pod-Checker
RUN mkdir /opt/epics \
    && cd /opt/epics \
    && curl http://www.aps.anl.gov/epics/download/base/baseR3.14.12.5.tar.gz | tar -xz \
    && mv base-3.14.12.5 base \
    && cd base \
    && make install
ENV PATH=/opt/epics/base/bin/linux-x86_64:$PATH \
    EPICS_BASE=/opt/epics/base \
    EPICS_HOST_ARCH=linux-x86_64
