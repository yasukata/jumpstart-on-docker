FROM ubuntu:24.04

ENV DPDK_VER=25.11
ENV DPDK_DIR=/dpdk
ENV DPDK_SRC_DIR=${DPDK_DIR}/dpdk-${DPDK_VER}
ENV DPDK_BUILD_DIR=${DPDK_SRC_DIR}/build
ENV DPDK_INSTALL_DIR=${DPDK_DIR}/install
ENV DPDK_MESON_LIBDIR=lib/dpdk

RUN apt update; apt install -y gcc make meson python3-pyelftools libnuma-dev
ADD https://fast.dpdk.org/rel/dpdk-${DPDK_VER}.tar.xz ${DPDK_DIR}/
RUN tar xvf ${DPDK_SRC_DIR}.tar.xz -C ${DPDK_DIR}
RUN if [ "`uname -m`x" = "aarch64x" ]; then meson -Dplatform=generic --prefix=${DPDK_INSTALL_DIR} --libdir=${DPDK_MESON_LIBDIR} ${DPDK_BUILD_DIR} ${DPDK_SRC_DIR}; elif  [ "`uname -m`x" = "x86_64x" ]; then meson --prefix=${DPDK_INSTALL_DIR} --libdir=${DPDK_MESON_LIBDIR} ${DPDK_BUILD_DIR} ${DPDK_SRC_DIR}; else echo "unhandled CPU type: `uname -m`"; exit 1; fi
RUN ninja -C ${DPDK_BUILD_DIR}
RUN ninja -C ${DPDK_BUILD_DIR} install

ENV OVS_VER=3.6.1
ENV OVS_DIR=/ovs
ENV OVS_SRC_DIR=${OVS_DIR}/ovs-${OVS_VER}
ENV OVS_INSTALL_DIR=${OVS_DIR}/install

RUN apt update; apt install -y autoconf libtool-bin pkg-config
ADD https://github.com/openvswitch/ovs/archive/refs/tags/v${OVS_VER}.tar.gz ${OVS_DIR}/
RUN tar xvf ${OVS_DIR}/v${OVS_VER}.tar.gz -C ${OVS_DIR}
RUN cd ${OVS_SRC_DIR}; ./boot.sh; export PKG_CONFIG_PATH=$PKG_CONFIG_PATH:${DPDK_INSTALL_DIR}/${DPDK_MESON_LIBDIR}/pkgconfig; export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:${DPDK_INSTALL_DIR}/${DPDK_MESON_LIBDIR}; ./configure --with-dpdk=static CFLAGS="-g -O3 -march=native" --prefix=${OVS_INSTALL_DIR} --localstatedir=${OVS_INSTALL_DIR}/var --sysconfdir=${OVS_INSTALL_DIR}/etc; make; make install
