FROM australiansynchrotron/epics:centos6
ENV PYTHON_VERSION=3.5.1
RUN yum -y install gcc-c++ curl-devel readline-devel openssl-devel ncurses-devel \
                   gdbm-devel sqlite-devel xz{,-devel} zlib-devel bzip2-devel \
                   tk-devel tcl-devel libjpeg-devel libpng-devel freetype-devel \
                   lapack-devel blas-devel \
    && cd /tmp \
    && curl -O https://www.python.org/ftp/python/$PYTHON_VERSION/Python-$PYTHON_VERSION.tar.xz \
    && tar -xf Python-$PYTHON_VERSION.tar.xz \
    && cd Python-$PYTHON_VERSION \
    && ./configure --with-ensurepip \
    && make install \
    && cd / \
    && rm -r /tmp/* \
    && yum clean all
