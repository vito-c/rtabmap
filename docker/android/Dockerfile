FROM %REPLACE_FROM%
MAINTAINER Vito

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

