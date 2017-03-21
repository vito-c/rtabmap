FROM ubuntu:latest
MAINTAINER Vito
################################################################################
# VTK
###############################################################################
WORKDIR /mnt/sources/dependencies
 # http://www.vtk.org/files/release/5.10/vtk-5.10.1.tar.gz
RUN curl -L http://www.vtk.org/files/release/6.3/VTK-6.3.0.tar.gz | tar -zx && \
     mv VTK-6.3.0 vtk
RUN mkdir -p /opt/${ANDROID_NDK_VERSION}/toolchains/arm-linux-androideabi-4.9/prebuilt/linux-x86_64/user

RUN mkdir /mnt/sources/dependencies/vtk/build 
WORKDIR /mnt/sources/dependencies/vtk/build

RUN cmake -DCMAKE_TOOLCHAIN_FILE=${ANDROID_CMAKE_TOOLCHAIN} \
     -DANDROID_NATIVE_API_LEVEL=${ANDROID_API_VERSION} -DVTK_ANDROID_BUILD=ON .. && \
     make -j2

