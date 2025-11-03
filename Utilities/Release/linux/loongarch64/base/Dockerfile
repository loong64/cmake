# Distributed under the OSI-approved BSD 3-Clause License.  See accompanying
# file LICENSE.rst or https://cmake.org/licensing for details.

# Produce a base image with a build environment for portable CMake binaries.
# Build using the directory containing this file as its own build context.

FROM ghcr.io/loong64/anolis:23

RUN : \
 && yum install --allowerasing -y \
        ca-certificates \
        curl \
        g++ \
        gcc \
        fontconfig-devel \
        freetype-devel \
        git \
        libstdc++-static \
        libX11-devel \
        libxcb-devel \
        make \
        patch \
        perl \
        perl-FindBin \
        python3-pip \
        xz \
        which \
 && yum clean all \
 && :
