FROM %REPLACE_FROM%
MAINTAINER Vito
################################################################################
# QHULL
###############################################################################
WORKDIR /mnt/sources/dependencies
RUN curl -L http://www.qhull.org/download/qhull-2015-src-7.2.0.tgz | tar -zx && \
    mv qhull-2015.2 qhull

RUN mkdir /mnt/sources/dependencies/qhull/output
WORKDIR /mnt/sources/dependencies/qhull/output

RUN cmake -DCMAKE_TOOLCHAIN_FILE=${ANDROID_CMAKE_TOOLCHAIN} \
    -DANDROID_NATIVE_API_LEVEL=${ANDROID_API_VERSION} .. && \
    make -j2

