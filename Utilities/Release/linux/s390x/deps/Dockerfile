# Distributed under the OSI-approved BSD 3-Clause License.  See accompanying
# file LICENSE.rst or https://cmake.org/licensing for details.

# Produce an image with custom-built dependencies for portable CMake binaries.
# Build using the directory containing this file as its own build context.

FROM ghcr.io/loong64/kitware/cmake:build-linux-s390x-base

# Sphinx
RUN : \
 && pip3 install -U pip \
 && pip3 install sphinx==2.1.2 \
 && :

# Curses
RUN : \
 && mkdir -p /opt/ncurses/src/ncurses-build \
 && cd /opt/ncurses/src \
 && curl -O https://ftp.gnu.org/pub/gnu/ncurses/ncurses-6.1.tar.gz \
 && sha512sum ncurses-6.1.tar.gz | grep -q e308af43f8b7e01e98a55f4f6c4ee4d1c39ce09d95399fa555b3f0cdf5fd0db0f4c4d820b4af78a63f6cf6d8627587114a40af48cfc066134b600520808a77ee \
 && tar xzf ncurses-6.1.tar.gz \
 && curl -Lo ncurses-6.1/config.guess "https://git.savannah.gnu.org/gitweb/?p=config.git;a=blob_plain;f=config.guess" \
 && curl -Lo ncurses-6.1/config.sub "https://git.savannah.gnu.org/gitweb/?p=config.git;a=blob_plain;f=config.sub" \
 && cd ncurses-build \
 && ../ncurses-6.1/configure \
      --prefix=/opt/ncurses \
      --with-terminfo-dirs=/etc/terminfo:/lib/terminfo:/usr/share/terminfo \
      --with-default-terminfo-dir=/usr/share/terminfo \
      --without-shared \
 && make -j $(nproc) \
 && make install.libs install.includes \
 && cd /opt \
 && rm -rf /opt/ncurses/src \
 && :

# OpenSSL
COPY openssl-source.patch /opt/openssl/src/
RUN : \
 && mkdir -p /opt/openssl/src \
 && cd /opt/openssl/src \
 && curl -OL https://github.com/openssl/openssl/releases/download/openssl-3.6.0/openssl-3.6.0.tar.gz \
 && sha256sum openssl-3.6.0.tar.gz | grep -q b6a5f44b7eb69e3fa35dbf15524405b44837a481d43d81daddde3ff21fcbb8e9 \
 && tar xzf openssl-3.6.0.tar.gz \
 && cd openssl-3.6.0 \
 && patch -p1 -i ../openssl-source.patch \
 && ./Configure --prefix=/opt/openssl linux-elf no-asm no-shared -D_POSIX_C_SOURCE=199506L -D_POSIX_SOURCE=1 -D_SVID_SOURCE=1 -D_BSD_SOURCE=1 \
 && make install_dev -j $(nproc) \
 && cd /opt \
 && rm -rf /opt/openssl/src \
 && :