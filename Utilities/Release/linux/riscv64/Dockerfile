# Distributed under the OSI-approved BSD 3-Clause License.  See accompanying
# file Copyright.txt or https://cmake.org/licensing for details.

# Produce an image containing a portable CMake binary package for Linux/riscv64.
# Build using the CMake source directory as the build context.
# The resulting image will have an '/out' directory containing the package.

# Keep this in sync with the `.gitlab/os-linux.yml` `.linux_release_riscv64` image.
FROM ghcr.io/loong64/kitware/cmake:build-linux-riscv64-deps

ARG VERSION

RUN : \
 && mkdir -p /opt/cmake/src/cmake \
 && git clone --depth=1 -b ${VERSION} https://github.com/Kitware/CMake /opt/cmake/src/cmake \
 && :

ARG TEST=true

COPY cache.txt /opt/cmake/src/cmake-build/CMakeCache.txt
RUN : \
 && cd /opt/cmake/src/cmake-build \
 && set -x \
 && ../cmake/bootstrap --parallel=$(nproc) --docdir=doc/cmake \
 && nice make -j $(nproc) \
 && if $TEST; then \
        # Run tests that require the full build tree.
        bin/ctest --output-on-failure -j 8 -R '^(CMake\.|CMakeLib\.|RunCMake\.ctest_memcheck)'; \
    fi \
 && bin/cpack -G TGZ \
 && bin/cpack -G STGZ \
 && set +x \
 && mkdir /out \
 && mv cmake-*-linux-riscv64.* /out \
 && :