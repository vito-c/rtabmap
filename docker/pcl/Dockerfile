FROM %REPLACE_FROM%
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
    -DLIBUSB_10_LIBRARY=/opt/libusb-1.0/libusb-1.0.a \
    -DLIBUSB_10_INCLUDE_DIR=/opt/libusb-1.0 \
    -DBUILD_global_tests=OFF \
    -DBUILD_people=OFF .. && \
    make -j2

