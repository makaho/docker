############################################################ 
# Dockerfile to build container images for discovardenovo
# Based on Ubuntu
############################################################ 

# Set the base image to Ubuntu
FROM ubuntu:14.04

# File Author / Maintainer
MAINTAINER Kathrin Klee <klee@uni-bonn.de>

# Update the repository sources list and install samtools
RUN apt-get update

# Install some utilities
RUN apt-get install --yes build-essential \
    gcc-multilib \
    apt-utils \
    zlib1g-dev \
    curl \
    python \
    python2.7-dev \
    python-dev \
    cmake \
    libboost-all-dev \
    git \
    wget \
    unzip

# install dependencies first
#RUN apt-get install -y glibc

# Change user to root
USER root

# Install jemalloc
ENV ZIP=jemalloc-4.5.0.tar.bz2
ENV URL=https://github.com/jemalloc/jemalloc/releases/download/4.5.0
ENV FOLDER=jemalloc-4.5.0
ENV DST=/tmp

RUN cd /tmp && \
    wget https://github.com/jemalloc/jemalloc/releases/download/4.5.0/jemalloc-4.5.0.tar.bz2 && \
    tar xvf jemalloc-4.5.0.tar.bz2 && \
    rm jemalloc-4.5.0.tar.bz2 && \
    cd jemalloc-4.5.0 && \
    ./configure --prefix=/usr/local && \
    make && \
    make install

# Install discovardenovo
ENV MALLOC_PER_THREAD 1

RUN cd /tmp && \
    wget ftp://ftp.broadinstitute.org/pub/crd/DiscovarDeNovo/latest_source_code/discovardenovo-52488.tar.gz && \
    tar xvf discovardenovo-52488.tar.gz && \
    rm discovardenovo-52488.tar.gz && \
    cd discovardenovo-52488 && \
    ./configure --prefix=/usr/local/ && \
    make all && \
    make install

RUN echo "\n\n export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib" >>~/.bashrc
# Change user to back to biodocker
#USER biodocker

# CHANGE WORKDIR TO /DATA
#WORKDIR /data
