# Simple notebook-based container image, unable to install MontagePy
# package in newer Ubuntu-based images, so pegged at Ubuntu 18 for now.
FROM jupyter/scipy-notebook:ubuntu-18.04

USER root
RUN apt-get update && apt-get install -y graphviz git

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
    && apt-get --yes install --no-install-recommends \
    bison \
    g++ \
    gfortran \
    git \
    libboost-python-dev \
    libboost-numpy-dev \
    vim

# Python3 requirements
COPY ./binder/requirements.txt /tmp/
RUN python3 -m pip install --upgrade pip
RUN python3 -m pip install -r /tmp/requirements.txt

# Add tests/source code
COPY ./ /opt/

# Fix permissions
RUN fix-permissions /opt/scripts
RUN fix-permissions /opt/ska
RUN fix-permissions /opt/tests

ENV LD_LIBRARY_PATH=${LD_LIBRARY_PATH}/usr/local/lib
ENV PYTHONPATH=${PYTHONPATH}:/opt

RUN python3 -m pip uninstall -y typing_extensions

WORKDIR $HOME
USER $NB_UID