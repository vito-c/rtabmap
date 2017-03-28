FROM ubuntu:latest
MAINTAINER Vito
################################################################################
# Make RTAB-MAP
###############################################################################
RUN mkdir -p /mnt/compiled/rtabmap/android_install
WORKDIR /mnt/sources/dependencies/
COPY rtabmap rtabmap
COPY ubuntu_bin /mnt/sources/dependencies/rtabmap/rtabmap-res_tool

