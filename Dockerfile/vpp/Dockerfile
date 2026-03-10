FROM ubuntu:24.04

RUN apt update; apt install -y curl
RUN curl -s https://packagecloud.io/install/repositories/fdio/release/script.deb.sh | bash
RUN apt update; apt install -y vpp vpp-plugin-core vpp-plugin-dpdk python3-vpp-api vpp-dbg vpp-dev

