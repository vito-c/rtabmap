FROM %REPLACE_FROM%
MAINTAINER Vito
################################################################################
# BOOST
###############################################################################
ENV BOOST_ANDROID=/opt

WORKDIR /mnt/sources/dependencies
RUN curl -L https://github.com/boostorg/boost/archive/boost-1.48.0.tar.gz | tar -zx
RUN mv boost-1.48.0 boost
# COPY libstdcpp3.hpp /mnt/sources/dependencies/boost/boost/config/stdlib/libstdcpp3.hpp
COPY user-config.jam /root/user-config.jam

#RUN curl -L https://github.com/moritz-wundke/Boost-for-Android/blob/master/build-android.sh
#COPY Boost-for-Android .
#RUN  chmod +x build-android.sh build-common.sh
#RUN mkdir -p /mnt/sources/dependencies/boost/logs
#WORKDIR /mnt/sources/dependencies/boost/logs && touch empty.txt 
#WORKDIR /mnt/sources/dependencies/boost
#RUN  ./build-android.sh ${NDK_ROOT} --boost=1.48.0

# Boost.IOStreams Bzip2 filters
# Boost.IOStreams Zlib filters
# Boost.MPI
# Boost.Python
# Boost.Regex ICU support

ENV BOOST_ROOT=/opt/boost
ENV BOOST_INCLUDEDIR=/opt/boost/include
ENV BOOST_LIBRARYDIR=/opt/boost/lib

RUN cd boost && \
    ./bootstrap.sh && \
    ./b2 --without-python -q \
    -s NO_BZIP2=1 \
    --prefix=/opt/boost  \
    variant=release link=static runtime-link=static \
    include=/opt/${ANDROID_NDK_VERSION}/platforms/${ANDROID_API_VERSION}/arch-arm/usr/include \
    cxxflags="-fexceptions -frtti -fpic -ffunction-sections -funwind-tables -D__ARM_ARCH_5__ -D__ARM_ARCH_5T__ -D__ARM_ARCH_5E__ -D__ARM_ARCH_5TE__ -Wno-psabi -march=armv5te -mtune=xscale -msoft-float -mthumb -Os -fomit-frame-pointer -fno-strict-aliasing -finline-limit=64 -Wa,--noexecstack -DANDROID -D__ANDROID__ -DNDEBUG -O2 -g" \
    install --libdir=${BOOST_LIBRARYDIR} toolset=gcc-android target-os=linux threading=multi; exit 0

#should we add that to cxxflags, need to verify those paths:
#-I$(AndroidNDKRoot)/sources/cxx-stl/gnu-libstdc++/4.6/include
#<compileflags>-I$(AndroidNDKRoot)/sources/cxx-stl/gnu-libstdc++/4.6/libs/armeabi/include

RUN ls -l ${BOOST_LIBRARYDIR}
RUN ls -l ${BOOST_ROOT}
RUN ls -l ${BOOST_INCLUDEDIR}/boost

