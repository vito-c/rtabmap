FROM ubuntu:latest
MAINTAINER Vito

RUN mkdir -p /mnt/scripts
RUN mkdir -p /mnt/sources

WORKDIR /opt

# I put this in the base image
# do not use the android-sdk downloaded with apt-get
RUN apt-get update && apt-get install -y \
    # libstdc++6:i386 \
    # libgcc1:i386 \
    # zlib1g:i386 \
    # libncurses5:i386 \
    android-sdk \
    cmake \
    curl \
    jam \
    lvtk-dev \
    unzip \
    gcc-multilib \
    vim \
    openjdk-8-jdk \
    freeglut3-dev \
    p7zip-full \
    && rm -rf /var/lib/apt/lists/*

###########################################################################################################
# ANDROID ENVS
###########################################################################################################
ENV ANDROID_NDK_VERSION="android-ndk-r10d"
ENV ANDROID_SDK_VERSION="android-sdk_r24.4.1"
# ENV ANDROID_SDK_VERSION="android-sdk_r21.1"

ENV ANDROID_HOME="/opt/android-sdk-linux"
ENV ANDROID_BUILD_TOOLS_REVISION="23.0.3"
ENV ANDROID_API_VERSION="android-21"
ENV ANDROID_NATIVE_API_LEVEL="android-21"
ENV ANDROID_TARGET_ARCH="armeabi-v7a"
ENV ANDROID_TOOLCHAIN_VERSION="4.9"
ENV TOOLCHAIN_ABI="arm-linux-androideabi-4.9"
ENV ANDROID_NDK_TOOLCHAIN_ROOT="/opt/pcl/android/${ANDROID_API_VERSION}-toolchain"
ENV ANDROID_STANDALONE_TOOLCHAIN="/opt/pcl/android/${ANDROID_API_VERSION}-toolchain"

# Paramertized ENVs
ENV ANDROID_NDK="/opt/${ANDROID_NDK_VERSION}"
ENV ANDROID_NDK_PATH="${ANDROID_NDK}"
ENV ANDROID_SDK_ROOT="${ANDROID_HOME}"
ENV ANDROID_SDK="${ANDROID_HOME}"
ENV NDKROOT="${ANDROID_NDK}"
ENV NDK_ROOT="/opt/${ANDROID_NDK_VERSION}"
ENV PATH=$PATH:"${ANDROID_NDK_TOOLCHAIN_ROOT}/arm-linux-androideabi/bin"
###########################################################################################################

# NDK Download
RUN curl -NL http://dl.google.com/android/ndk/${ANDROID_NDK_VERSION}-linux-x86_64.bin  -o ./ndk.bin && \
 7z x  ndk.bin && \
 rm -rf ndk.bin

# SDK Download
RUN curl -L https://dl.google.com/android/${ANDROID_SDK_VERSION}-linux.tgz | tar -zx 

# Android SDK install
RUN echo y | ${ANDROID_HOME}/tools/android update sdk \
 --no-ui -a --filter platform-tools,${ANDROID_API_VERSION},build-tools-${ANDROID_BUILD_TOOLS_REVISION} && chmod -R 755 ${ANDROID_HOME}

RUN exit 0

ENV PATH=$PATH:"${ANDROID_NDK_TOOLCHAIN_ROOT}/bin"

# ENV PATH=$PATH:"/mnt/sources/android-ndk-r12b"
# ENV PATH=$PATH:"/usr/lib/android-sdk/build-tools"
# ENV PATH=$PATH:"/usr/lib/android-sdk/build-tools/bin"
# ENV PATH=$PATH:"/usr/lib/android-sdk/platform-tools"
# ENV PATH=$PATH:"/usr/lib/android-sdk/tools"

RUN ${ANDROID_NDK}/build/tools/make-standalone-toolchain.sh \
    --system=linux-x86_64 \
    --platform=${ANDROID_API_VERSION} \
    --ndk-dir=${ANDROID_NDK} \
    --install-dir=${ANDROID_NDK_TOOLCHAIN_ROOT} \
    --toolchain=${TOOLCHAIN_ABI}

ENV ANDROID_CMAKE=/opt/android-cmake
ENV ANDROID_CMAKE_TOOLCHAIN=/opt/android-cmake/android.toolchain.cmake
ENV ANDTOOLCHAIN=/opt/android-cmake/android.toolchain.cmake

RUN curl -L https://github.com/vito-c/android-cmake/archive/ndk-12r.tar.gz | tar -xz && \
    mv android-cmake-ndk-12r android-cmake


# export ANDROID_CMAKE=/work/PCL/pcl/trunk/android/android_dependencies/android-cmake
# export ANDTOOLCHAIN=/work/PCL/pcl/trunk/android/android_dependencies/android-cmake/toolchain/android.toolchain.cmake
# alias android-cmake='cmake -DCMAKE_TOOLCHAIN_FILE=/work/PCL/pcl/trunk/android/android_dependencies/android-cmake/toolchain/android.toolchain.cmake 
# -DCMAKE_INSTALL_NAME_TOOL=/usr/bin/install_name_tool '
#
# pcl_trunk$ mkdir build_android && cd build_android
# cmake -DCMAKE_TOOLCHAIN_FILE=/work/PCL/pcl/trunk/android/android_dependencies/android-cmake/toolchain/android.toolchain.cmake 
# -DCMAKE_INSTALL_NAME_TOOL=/usr/bin/install_name_tool
# -DANDROID_API_LEVEL=9 
# -DCMAKE_INSTALL_PREFIX=/work/PCL/Android/android-ndk-r6b/toolchains/arm-linux-androideabi-4.4.3/prebuilt/linux-x86/user/ 
# -DBUILD_visualization=OFF 
# -DBUILD_global_tests=OFF 
# -DBUILD_people=OFF ..

# cmake -DCMAKE_TOOLCHAIN_FILE=../cmake_modules/android.toolchain.cmake -DBUILD_SHARED_LIBS=OFF -DBUILD_TOOLS=OFF -DCMAKE_BUILD_TYPE=Release ..
# cmake -DOPENNI_INCLUDE_DIRS=/usr/local/Cellar/openni/1.5.7.10/include -DBOOST_LIBRARYDIR=/Users/vitocutten/playground/pcl-superbuild-with-release/build/CMakeExternals/Install/boost-android/lib -DBoost_INCLUDE_DIR=/Users/vitocutten/playground/pcl-superbuild-with-release/build/CMakeExternals/Install/boost-android/include -DBoost_DEBUG=ON -DPCL_VERSION=1.7.1 -DEIGEN_INCLUDE_DIR=$DEP_FOLDER/eigen -CMAKE_TOOLCHAIN_FILE=$ANDTOOLCHAIN ..

# CMD ["cmake", "-DCMAKE_TOOLCHAIN_FILE=${ANDROID_CMAKE_TOOLCHAIN}", "-DANDROID_API_LEVEL=9", "-DBUILD_visualization=OFF", "-DBUILD_global_tests=OFF", "-DBUILD_people=OFF", ".."]
# -DBOOST_LIBRARYDIR= \
# -DBoost_INCLUDE_DIR= /usr/\
# Boost : 1.48.0
# Flann : 1.7.1
# Qhull : 2011.1
# Qt    : 4.8.0
# VTK   : 5.8.0
# GTest : 1.6.0
# C:/PCL_dependencies
# C:/PCL_dependencies/boost-cmake
# C:/PCL_dependencies/eigen
# C:/PCL_dependencies/flann-1.7.1-src
# C:/PCL_dependencies/gtest-1.6.0
# C:/PCL_dependencies/qhull
# C:/PCL_dependencies/VTK

RUN mkdir -p /mnt/sources/dependencies
RUN mkdir -p /mnt/compiled/dependencies

################################################################################
# EIGEN
###############################################################################
ENV EIGEN_ROOT=/mnt/sources/dependencies/eigen
WORKDIR /mnt/sources/dependencies
RUN curl -L http://bitbucket.org/eigen/eigen/get/3.2.9.tar.gz | tar -zx && \
    mv eigen-eigen-dc6cfdf9bcec eigen

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

################################################################################
# QT
###############################################################################
# configure.bat -developer-build \ 
# -platform win32-g++ \
# -opengl es2 -xplatform \
# android-g++ 
# -android-ndk ANDROID_NDK_PATH 
# -android-sdk ANDROID_SDK_ROOT 
# -opensource -confirm-license -nomake tests -nomake examples
# mingw32-make.exe<N>
WORKDIR /mnt/sources/dependencies
RUN curl -L https://download.qt.io/official_releases/qt/5.7/5.7.0/single/qt-everywhere-opensource-src-5.7.0.tar.gz | tar -zx && \
    mv qt-everywhere-opensource-src-5.7.0 qt

WORKDIR /mnt/sources/dependencies/qt
RUN ./configure -opensource -confirm-license \
-xplatform android-g++ \
-nomake tests \
-nomake examples \
-android-ndk ${ANDROID_NDK} \
-android-sdk ${ANDROID_SDK} \
-android-ndk-host linux-x86_64 \
-android-toolchain-version 4.9 \
-skip qttranslations \
-skip qtserialport \
-no-warnings-are-errors \
&& make -j2

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


# RUN ./configure -xplatform android-g++ \
#   -nomake tests -nomake examples \
#   -android-ndk <path/to/ndk> 
#   -android-sdk <path/to/sdk> 
#   -android-ndk-host <e.g. linux-x86_64> 
#   -android-toolchain-version <e.g. 4.8> 
#   -skip qttranslations -skip qtwebkit 
#   -skip qtserialport 
#   -skip qtwebkit-examples 
#   -no-warnings-are-errors

# FLANN_LIB_INSTALL_DIR
# FLANN_INCLUDE_INSTALL_DIR
#-DANDROID_NATIVE_API_LEVEL=android-17

# RUN b2 --reconfigure \
#     toolset=gcc-arm \
#     include=${ANDROID_NDK}/sources/cxx-stl/gnu-libstdc++/4.9/include \
#     include=${ANDROID_NDK}/sources/cxx-stl/gnu-libstdc++/4.9/libs/armeabi-v7a/include \
#     include=${ANDROID_NDK}/platforms/${ANDROID_API_VERSION}/arch-arm/usr/include \
#     install --libdir=stage/lib/armeabi-v7a




# RUN mkdir /mnt/sources/pcl/build
# WORKDIR /mnt/sources/pcl/build
# RUN cmake -DCMAKE_TOOLCHAIN_FILE=${ANDROID_CMAKE_TOOLCHAIN} \
#     -DFLANN_INCLUDE_DIR=/usr/include/flann \
#     -DEIGEN_INCLUDE_DIR=/usr/include/eigen3 \
#     -DBUILD_SHARED_LIBS=OFF \
#     -DANDROID_API_LEVEL=9 \
#     -DBUILD_visualization=OFF \
#     -DBUILD_global_tests=OFF \
#     -DBUILD_people=OFF \
#     ..
#

################################################################################
# BOOST
###############################################################################
ENV BOOST_ANDROID=/mnt/compiled/dependencies/boost
WORKDIR /mnt/sources/dependencies
COPY boost_1_48_0 boost
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

ENV BOOST_ROOT=/mnt/compiled/dependencies/boost
ENV BOOST_INCLUDEDIR=/mnt/compiled/dependencies/boost/include
ENV BOOST_LIBRARYDIR=/mnt/compiled/dependencies/boost/lib

RUN cd boost && \
    ./bootstrap.sh && \
    ./b2 --without-python -q \
    -s NO_BZIP2=1 \
    --prefix=/mnt/compiled/dependencies/boost  \
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

################################################################################
# LibUSB
################################################################################
WORKDIR /mnt/sources/dependencies/
RUN echo building libusb
COPY libusb-master.zip libusb-master.zip
RUN unzip libusb-master.zip
RUN rm -rf libusb-master.zip
RUN mv libusb-master libusb
WORKDIR /mnt/sources/dependencies/libusb/android
RUN ${ANDROID_NDK_PATH}/ndk-build
RUN mkdir /mnt/compiled/dependencies/libusb-1.0
#copy the generared static lib to a nicer location:
RUN cp /mnt/sources/dependencies/libusb/android/obj/local/armeabi/libusb1.0.a /mnt/compiled/dependencies/libusb-1.0/libusb-1.0.a
RUN mkdir /mnt/compiled/dependencies/libusb-1.0/libusb-1.0
RUN cp /mnt/sources/dependencies/libusb/libusb/libusb.h /mnt/compiled/dependencies/libusb-1.0/libusb-1.0/libusb.h
################################################################################
# Make PCL
###############################################################################

WORKDIR /mnt/sources/dependencies/
#RUN curl -L https://github.com/PointCloudLibrary/pcl/archive/pcl-1.7.2.tar.gz | tar -xz
#mv pcl-pcl-1.7.2 pcl
COPY pcl-pcl-1.7.2 pcl
RUN mkdir -p /mnt/sources/dependencies/pcl/build-android
WORKDIR /mnt/sources/dependencies/pcl/build-android
RUN ${android-cmake} --no-warn -DANDROID_API_LEVEL=${ANDROID_API_VERSION} \
    -DOPENGL_gl_LIBRARY=/opt/android-ndk-r10d/platforms/android-21/arch-arm/usr/lib/libGLESv3.so \
    -DOPENGL_INCLUDE_DIR=/opt/android-ndk-r10d/platforms/android-21/arch-arm/usr/include/gles3 \
    -DCMAKE_TOOLCHAIN_FILE=${ANDROID_CMAKE_TOOLCHAIN} \
    -DCMAKE_INSTALL_PREFIX=/opt/${ANDROID_NDK_VERSION}/toolchains/arm-linux-androideabi-4.9/prebuilt/linux-x86_64/user \
    -DHAVE_POSIX_MEMALIGN_EXITCODE=0 \
    -DBUILD_visualization=OFF \
    -DPCL_SHARED_LIBS=OFF \
    -DBoost_DEBUG=ON \
    -DBoost_USE_STATIC_LIBS=ON \
    -DBoost_USE_MULTITHREADED=ON \
    -DBoost_LIBRARY_DIR=${BOOST_LIBRARYDIR} \
    -DBoost_INCLUDE_DIR=${BOOST_INCLUDEDIR} \
    -DQHULL_LIBRARY=/mnt/sources/dependencies/qhull/output/libqhull.so \
    -DQHULL_INCLUDE_DIR=/mnt/sources/dependencies/qhull/src \
    -DEIGEN_INCLUDE_DIR=${EIGEN_ROOT} \
    -DFLANN_INCLUDE_DIR=/mnt/sources/dependencies/flann/src/cpp \
    -DBUILD_SHARED_LIBS=OFF \
    -DFLANN_LIBRARY=${FLANN_LIBRARY} \
    -DLIBUSB_10_LIBRARY=/mnt/compiled/dependencies/libusb-1.0/libusb-1.0.a \
    -DLIBUSB_10_INCLUDE_DIR=/mnt/compiled/dependencies/libusb-1.0 \
    -DBUILD_global_tests=OFF \
    -DBUILD_people=OFF .. && \
    make -j2

################################################################################
# OPEN CV
###############################################################################
WORKDIR /mnt/compiled/dependencies/
COPY openCV openCV

# TODO: STOPPED HERE
################################################################################
# Make RTAB-MAP
###############################################################################
RUN mkdir -p /mnt/compiled/rtabmap/android_install
WORKDIR /mnt/sources/dependencies/
COPY rtabmap rtabmap
COPY ubuntu_bin /mnt/sources/dependencies/rtabmap/rtabmap-res_tool

#COPY ./TangoSDK_Yildun_C.zip TangoSDK.zip
COPY  ./TangoSDK_Zaniah_C.zip   Tango_C.zip
COPY ./TangoSDK_Zaniah_Java.jar  /mnt/sources/dependencies/rtabmap/build/app/android/libs/Tango_java.jar

#RUN unzip TangoSDK.zip
RUN unzip Tango_C.zip

RUN mkdir ~/android_install

WORKDIR /mnt/sources/dependencies/eigen
RUN mkdir build && cd build && cmake .. && make install

WORKDIR /mnt/sources/dependencies/rtabmap/build

RUN apt-get update && apt-get install -y ant lib32z1 \
    && rm -rf /var/lib/apt/lists/*

# /mnt/sources/dependencies/pcl/apps/cloud_composer/include
# /mnt/sources/dependencies/pcl/apps/in_hand_scanner/include
# /mnt/sources/dependencies/pcl/apps/include
# /mnt/sources/dependencies/pcl/apps/modeler/include
# /mnt/sources/dependencies/pcl/apps/optronic_viewer/include
# /mnt/sources/dependencies/pcl/apps/point_cloud_editor/include
# /mnt/sources/dependencies/pcl/build-android/include
# /mnt/sources/dependencies/pcl/doc/tutorials/content/sources/iccv2011/include
# /mnt/sources/dependencies/pcl/doc/tutorials/content/sources/iros2011/include
# /mnt/sources/dependencies/pcl/outofcore/include
# /mnt/sources/dependencies/pcl/people/include
# /mnt/sources/dependencies/pcl/visualization
# ../../pcl/build-android/include
    ENV PCL_COMMON_LIBRARY=/mnt/sources/dependencies/pcl/build-android/lib/libpcl_common.a
    ENV PCL_OCTREE_LIBRARY=/mnt/sources/dependencies/pcl/build-android/lib/libpcl_octree.a
    ENV PCL_IO_LIBRARY=/mnt/sources/dependencies/pcl/build-android/lib/libpcl_io.a
    ENV PCL_KDTREE_LIBRARY=/mnt/sources/dependencies/pcl/build-android/lib/libpcl_kdtree.a
    ENV PCL_SEARCH_LIBRARY=/mnt/sources/dependencies/pcl/build-android/lib/libpcl_search.a
    ENV PCL_SAMPLE_CONSENSUS_LIBRARY=/mnt/sources/dependencies/pcl/build-android/lib/libpcl_sample_consensus.a
    ENV PCL_FILTERS_LIBRARY=/mnt/sources/dependencies/pcl/build-android/lib/libpcl_filters.a
    ENV PCL_FEATURES_LIBRARY=/mnt/sources/dependencies/pcl/build-android/lib/libpcl_features.a
    ENV PCL_REGISTRATION_LIBRARY=/mnt/sources/dependencies/pcl/build-android/lib/libpcl_registration.a
    ENV PCL_RECOGNITION_LIBRARY=/mnt/sources/dependencies/pcl/build-android/lib/libpcl_recognition.a
    ENV PCL_SEGMENTATION_LIBRARY=/mnt/sources/dependencies/pcl/build-android/lib/libpcl_segmentation.a
    ENV PCL_SURFACE_LIBRARY=/mnt/sources/dependencies/pcl/build-android/lib/libpcl_surface.a
    ENV PCL_KEYPOINTS_LIBRARY=/mnt/sources/dependencies/pcl/build-android/lib/libpcl_keypoints.a
    ENV PCL_TRACKING_LIBRARY=/mnt/sources/dependencies/pcl/build-android/lib/libpcl_tracking.a
    ENV PCL_IO_PLY_UNUSED=/mnt/sources/dependencies/pcl/build-android/lib/libpcl_io_ply.a
    ENV PCL_COMMON_INCLUDE_DIR=/mnt/sources/dependencies/pcl/common/include
    ENV PCL_OCTREE_INCLUDE_DIR=/mnt/sources/dependencies/pcl/octree/include
    ENV PCL_IO_INCLUDE_DIR=/mnt/sources/dependencies/pcl/io/include
    ENV PCL_KDTREE_INCLUDE_DIR=/mnt/sources/dependencies/pcl/kdtree/include
    ENV PCL_SEARCH_INCLUDE_DIR=/mnt/sources/dependencies/pcl/search/include
    ENV PCL_SAMPLE_CONSENSUS_INCLUDE_DIR=/mnt/sources/dependencies/pcl/sample_consensus/include
    ENV PCL_FILTERS_INCLUDE_DIR=/mnt/sources/dependencies/pcl/filters/include
    ENV PCL_FEATURES_INCLUDE_DIR=/mnt/sources/dependencies/pcl/features/include
    ENV PCL_REGISTRATION_INCLUDE_DIR=/mnt/sources/dependencies/pcl/registration/include
    ENV PCL_RECOGNITION_INCLUDE_DIR=/mnt/sources/dependencies/pcl/recognition/include
    ENV PCL_GEOMETRY_INCLUDE_DIR=/mnt/sources/dependencies/pcl/geometry/include
    ENV PCL_SEGMENTATION_INCLUDE_DIR=/mnt/sources/dependencies/pcl/segmentation/include
    ENV PCL_SURFACE_INCLUDE_DIR=/mnt/sources/dependencies/pcl/surface/include
    ENV PCL_KEYPOINTS_INCLUDE_DIR=/mnt/sources/dependencies/pcl/keypoints/include
    ENV PCL_TRACKING_INCLUDE_DIR=/mnt/sources/dependencies/pcl/tracking/include

    ENV QHULL_LIBRARY=/mnt/sources/dependencies/qhull/output/libqhullstatic.a
    ENV QHULL_INCLUDE_DIRS=/mnt/sources/dependencies/qhull/src


    ENV OPENNI_LIBRARY=
    ENV OPENNI_INCLUDE_DIRS=
    ENV OPENNI2_LIBRARY=
    ENV OPENNI2_INCLUDE_DIRS=
    # -DCMAKE_INSTALL_PREFIX=/mnt/compiled/rtabmap/android_install \
    # -DCMAKE_INSTALL_PREFIX=/opt/${ANDROID_NDK_VERSION}/toolchains/arm-linux-androideabi-4.9/prebuilt/linux-x86_64/user \

# -DBUILD_SHARED_LIBS=OFF -DBUILD_TOOLS=OFF -DCMAKE_BUILD_TYPE=Release \ 
# -DWITH_QHULL:BOOL=OFF     -DWITH_PCAP:BOOL=OFF     -DWITH_PNG:BOOL=OFF \ 
# -DWITH_OPENGL:BOOL=OFF     -DWITH_LIBUSB:BOOL=OFF -DWITH_OPENNI:BOOL=OFF \ 
# -DWITH_OPENNI2:BOOL=OFF -DBUILD_visualization:BOOL=OFF -DBUILD_examples:BOOL=OFF \ 
# -DBoost_INCLUDE_DIR=/opt/android-toolchain/arm-linux-androideabi/include \ 
# -DBoost_LIBRARY_DIR_DEBUG=/opt/android-toolchain/arm-linux-androideabi/lib  \ 
# -DBoost_LIBRARY_DIR_RELEASE=/opt/android-toolchain/arm-linux-androideabi/lib  \ 
# -DHAVE_POSIX_MEMALIGN_EXITCODE=OFF \ 
# -DOPENGL_gl_LIBRARY=/usr/lib/x86_64-linux-gnu \ 
# -DOPENGL_INCLUDE_DIR=/usr/include/x86_64-linux-gnu \ 
# -DEIGEN_INCLUDE_DIR=/opt/android-toolchain/arm-linux-androideabi/include/eigen3 \ 
# -DFLANN_INCLUDE_DIR=/opt/android-toolchain/arm-linux-androideabi/include/flann \ 
# -DFLANN_LIBRARY:FILEPATH=/opt/android-toolchain/arm-linux-androideabi/lib/libflann_cpp_s.a 


RUN ${android-cmake} \
    -DANDROID_NATIVE_API_LEVEL=android-21 \
    -DOpenCV_DIR=/mnt/compiled/dependencies/openCV/sdk/native/jni \
    -DCMAKE_TOOLCHAIN_FILE=${ANDROID_CMAKE_TOOLCHAIN} \
    -DBUILD_SHARED_LIBS=OFF -DBUILD_TOOLS=OFF \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_APP_CPPFLAGS=-std=c++11 \
    -DEIGEN_INCLUDE_DIRS=/usr/local/include/eigen3 \
    -DPCL_DIR=/mnt/sources/dependencies/pcl/build-android \
    -DFLANN_LIBRARY=${FLANN_LIBRARY} \
    -DFLANN_INCLUDE_DIRS=/opt/android-ndk-r10d/toolchains/arm-linux-androideabi-4.9/prebuilt/linux-x86_64/user/include \
    -DRTABMAP_RES_TOOL=/mnt/sources/dependencies/rtabmap/rtabmap-res_tool/rtabmap-res_tool-0.3.0_ubuntu \
    -DTango_LIBRARY=/mnt/sources/dependencies/lib_tango_client_api/armeabi-v7a/libtango_client_api.so \
    -DTango_INCLUDE_DIR=/mnt/sources/dependencies/lib_tango_client_api \
    -DBoost_USE_STATIC_LIBS=ON \
    -DBoost_USE_MULTITHREADED=ON \
    -DBoost_LIBRARY_DIR=${BOOST_LIBRARYDIR} \
    -DBoost_INCLUDE_DIR=${BOOST_INCLUDEDIR} \
    -DOPENNI_LIBRARY=${OPENNI_LIBRARY} \
    -DOPENNI_INCLUDE_DIRS=${OPENNI_INCLUDE_DIRS} \
    -DOPENNI2_LIBRARY=${OPENNI2_LIBRARY} \
    -DOPENNI2_INCLUDE_DIRS=${OPENNI2_INCLUDE_DIRS} \
    -DQHULL_LIBRARY=${QHULL_LIBRARY} \
    -DQHULL_INCLUDE_DIRS=${QHULL_INCLUDE_DIRS} \
    -DPCL_COMMON_LIBRARY=${PCL_COMMON_LIBRARY} \
    -DPCL_COMMON_INCLUDE_DIR=${PCL_COMMON_INCLUDE_DIR} \
    -DPCL_OCTREE_LIBRARY=${PCL_OCTREE_LIBRARY} \
    -DPCL_OCTREE_INCLUDE_DIR=${PCL_OCTREE_INCLUDE_DIR} \
    -DPCL_IO_INCLUDE_DIR=${PCL_IO_INCLUDE_DIR} \
    -DPCL_IO_LIBRARY=${PCL_IO_LIBRARY} \
    -DPCL_KDTREE_LIBRARY=${PCL_KDTREE_LIBRARY} \
    -DPCL_KDTREE_INCLUDE_DIR=${PCL_KDTREE_INCLUDE_DIR} \
    -DPCL_SEARCH_LIBRARY=${PCL_SEARCH_LIBRARY} \
    -DPCL_SEARCH_INCLUDE_DIR=${PCL_SEARCH_INCLUDE_DIR} \
    -DPCL_SAMPLE_CONSENSUS_LIBRARY=${PCL_SAMPLE_CONSENSUS_LIBRARY} \
    -DPCL_SAMPLE_CONSENSUS_INCLUDE_DIR=${PCL_SAMPLE_CONSENSUS_INCLUDE_DIR} \
    -DPCL_FILTERS_LIBRARY=${PCL_FILTERS_LIBRARY} \
    -DPCL_FILTERS_INCLUDE_DIR=${PCL_FILTERS_INCLUDE_DIR} \
    -DPCL_FEATURES_LIBRARY=${PCL_FEATURES_LIBRARY} \
    -DPCL_FEATURES_INCLUDE_DIR=${PCL_FEATURES_INCLUDE_DIR} \
    -DPCL_REGISTRATION_LIBRARY=${PCL_REGISTRATION_LIBRARY} \
    -DPCL_REGISTRATION_INCLUDE_DIR=${PCL_REGISTRATION_INCLUDE_DIR} \
    -DPCL_RECOGNITION_LIBRARY=${PCL_RECOGNITION_LIBRARY} \
    -DPCL_RECOGNITION_INCLUDE_DIR=${PCL_RECOGNITION_INCLUDE_DIR} \
    -DPCL_GEOMETRY_INCLUDE_DIR=${PCL_GEOMETRY_INCLUDE_DIR} \
    -DPCL_SEGMENTATION_LIBRARY=${PCL_SEGMENTATION_LIBRARY} \
    -DPCL_SEGMENTATION_INCLUDE_DIR=${PCL_SEGMENTATION_INCLUDE_DIR} \
    -DPCL_SURFACE_LIBRARY=${PCL_SURFACE_LIBRARY} \
    -DPCL_SURFACE_INCLUDE_DIR=${PCL_SURFACE_INCLUDE_DIR} \
    -DPCL_KEYPOINTS_LIBRARY=${PCL_KEYPOINTS_LIBRARY} \
    -DPCL_KEYPOINTS_INCLUDE_DIR=${PCL_KEYPOINTS_INCLUDE_DIR} \
    -DPCL_TRACKING_LIBRARY=${PCL_TRACKING_LIBRARY} \
    -DPCL_TRACKING_INCLUDE_DIR=${PCL_TRACKING_INCLUDE_DIR} \
    -DPCL_IO_PLY_UNUSED=${PCL_IO_PLY_UNUSED} \
    -DANDROID_SYSROOT=/opt/pcl/android/android-21-toolchain/sysroot \
    -DANDROID_NATIVE_API_LEVEL=android-21 \
    -DANDROID_STANDALONE_TOOLCHAIN="/opt/pcl/android/${ANDROID_API_VERSION}-toolchain" \
    -DANDROID_EXECUTABLE=/opt/android-sdk-linux/tools/android \
    ..
    # -DCSPARSE_INCLUDE_DIR=${CSPARSE_INCLUDE_DIR} \
    # -DCSPARSE_LIBRARY=${CSPARSE_LIBRARY} \


RUN chmod +x /mnt/sources/dependencies/rtabmap/rtabmap-res_tool/rtabmap-res_tool-0.3.0_ubuntu
RUN ln -s /mnt/sources/dependencies/rtabmap/rtabmap-res_tool/librtabmap_utilite.so.0.11.11 /mnt/sources/dependencies/rtabmap/rtabmap-res_tool/librtabmap_utilite.so.0.11
RUN cp /mnt/sources/dependencies/rtabmap/rtabmap-res_tool/*.* /usr/local/lib/
RUN cp /mnt/sources/dependencies/lib_tango_client_api/armeabi-v7a/libtango_client_api.so /mnt/sources/dependencies/rtabmap/build/app/android/libs/armeabi-v7a/.
RUN ldconfig /usr/local/lib

# RUN make -j2
# -DTango_LIBRARY=/mnt/sources/dependencies/lib_tango_client_api \
# -DTango_INCLUDE_DIRS=/mnt/sources/dependencies/lib_tango_client_api/armeabi-v7a \
#     -DOPENNI_LIBRARY 
#     -DOPENNI_INCLUDE_DIRS
#     -DOPENNI2_LIBRARY 
    # -DOPENNI2_INCLUDE_DIRS
    # -DT=/mnt/sources/dependencies/lib_tango_client_api \
#
#  ${android-cmake} -DOpenCV_DIR=/mnt/compiled/dependencies/openCV/sdk/native/jni -DCMAKE_TOOLCHAIN_FILE=../cmake_modules/android.toolchain.cmake -DBUILD_SHARED_LIBS=OFF -DBUILD_TOOLS=OFF -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/mnt/compiled/dependencies/ -DPCL_DIR=/mnt/sources/dependencies/pcl/build-android ..
