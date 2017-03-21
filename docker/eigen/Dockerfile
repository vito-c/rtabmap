FROM ubuntu:latest
MAINTAINER Vito
################################################################################
# EIGEN
###############################################################################
ENV EIGEN_ROOT=/mnt/sources/dependencies/eigen
WORKDIR /mnt/sources/dependencies
RUN curl -L http://bitbucket.org/eigen/eigen/get/3.2.9.tar.gz | tar -zx && \
    mv eigen-eigen-dc6cfdf9bcec eigen
