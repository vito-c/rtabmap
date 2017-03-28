FROM %REPLACE_FROM%
MAINTAINER Vito

################################################################################
# FLANN
###############################################################################
ENV FLANN_ROOT=/mnt/sources/dependencies/flann/build
WORKDIR /mnt/sources/dependencies
RUN curl -L http://www.cs.ubc.ca/research/flann/uploads/FLANN/flann-1.8.4-src.zip > ./flann.zip && \
   unzip flann.zip && \
   mv flann-1.8.4-src flann && \
   rm -rf flann.zip
ENV FLANN_LIBRARY=${FLANN_ROOT}/lib/libflann_s.a
# COPY flann-1.8.4-src flann

RUN mkdir /mnt/sources/dependencies/flann/build 
WORKDIR /mnt/sources/dependencies/flann/build

# RUN ed -s text.txt <<'EOF' \
# 1i \
# line 1 \
# . \
# w \
# EOF
RUN cmake -DCMAKE_TOOLCHAIN_FILE=${ANDROID_CMAKE_TOOLCHAIN} \
    -DANDROID_NATIVE_API_LEVEL=${ANDROID_API_VERSION} .. && \
    make -j2
RUN make install

