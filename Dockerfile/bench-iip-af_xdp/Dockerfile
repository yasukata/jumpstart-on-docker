FROM ubuntu:24.04

RUN apt update; apt install -y unzip gcc make libnuma-dev libxdp-dev

ADD https://github.com/yasukata/bench-iip/archive/refs/heads/master.zip ./
RUN unzip ./master.zip; mv bench-iip-master bench-iip; rm master.zip
ADD https://github.com/yasukata/iip/archive/refs/heads/master.zip ./
RUN unzip ./master.zip; mv iip-master ./bench-iip/iip; rm master.zip
ADD https://github.com/yasukata/iip-af_xdp/archive/refs/heads/master.zip ./
RUN unzip ./master.zip; mv iip-af_xdp-master ./bench-iip/iip-af_xdp; rm master.zip

RUN mv /bench-iip/iip-af_xdp/main.c /bench-iip/iip-af_xdp/_main.c; echo "#define helper_ip4_get_connection_affinity __helper_ip4_get_connection_affinity" > /bench-iip/iip-af_xdp/main.c; echo "#include \"_main.c\"" >> /bench-iip/iip-af_xdp/main.c; echo "#undef helper_ip4_get_connection_affinity" >> /bench-iip/iip-af_xdp/main.c; echo "static uint16_t helper_ip4_get_connection_affinity(uint16_t protocol, uint32_t local_ip4_be, uint16_t local_port_be, uint32_t peer_ip4_be, uint16_t peer_port_be, void *opaque) { return 0; (void) protocol; (void) local_ip4_be; (void) local_port_be; (void) peer_ip4_be; (void) peer_port_be; (void) opaque; (void) __helper_ip4_get_connection_affinity; }" >> /bench-iip/iip-af_xdp/main.c
RUN cd bench-iip; IOSUB_DIR=./iip-af_xdp make
