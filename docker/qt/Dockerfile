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

