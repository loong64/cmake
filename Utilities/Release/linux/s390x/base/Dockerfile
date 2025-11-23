# Distributed under the OSI-approved BSD 3-Clause License.  See accompanying
# file LICENSE.rst or https://cmake.org/licensing for details.

# Produce a base image with a build environment for portable CMake binaries.
# Build using the directory containing this file as its own build context.

FROM quay.io/almalinuxorg/almalinux:8

RUN : \
 && yum -y install dnf-plugins-core \
 && yum config-manager --set-enabled powertools \
 && yum install --allowerasing -y \
        ca-certificates \
        curl \
        gcc-c++ \
        fontconfig-devel \
        freetype-devel \
        git \
        libstdc++-static \
        libX11-devel \
        libxcb-devel \
        make \
        patch \
        perl \
        python3-pip \
        xz \
        which \
 && yum clean all \
 && :

# Clang
ARG RUNNER_ARCH
RUN : \
 && cd /opt \
 && curl -LO https://github.com/loong64/static-clang-build/raw/refs/heads/main/install-static-clang.sh \
 && bash ./install-static-clang.sh \
 && rm -f ./install-static-clang.sh \
 && :

ENV PATH="/opt/clang/bin:$PATH"
