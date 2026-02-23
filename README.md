# A Collection of Docker-based Start Kits

This repository aims to showcase the minimal setups required to launch applications.

# Disclaimer

The authors disclaim all responsibility for any problems resulting from the contents of this repository.

# Assumed setup

- Number of CPU cores: 4
- DRAM size: 8 GB
- OS: Linux

# Preliminary settings

### Huge pages configuration for DPDK

```
sudo sh -c "echo 2048 > /sys/kernel/mm/hugepages/hugepages-2048kB/nr_hugepages"
```

### Open vSwitch kernel module installation for OVS-DPDK

```
sudo modprobe openvswitch
```

# Contents

## DPDK installation

- [Dockerfile/dpdk](Dockerfile/dpdk)
- [compose.yaml/basic/dpdk](compose.yaml/basic/dpdk)
- Two containers run the testpmd application: one serves as the sender, and the other acts as the receiver. The two containers are directly connected via the virtio/vhost-user interface.
- [Huge pages configuration](#huge-pages-configuration-for-dpdk) is necessary.

```
docker compose -f jumpstart-on-docker/compose.yaml/basic/dpdk/compose.yaml up
```

## OVS-DPDK installation

- [Dockerfile/ovs-dpdk](Dockerfile/ovs-dpdk)
- [compose.yaml/basic/ovs-dpdk](compose.yaml/basic/ovs-dpdk)
- Two containers, running testpmd, send and receive data over an OVS-DPDK switch, which is run by another container. The containers are connected via the virtio/vhost-user interfaces.
- [Huge pages configuration](#huge-pages-configuration-for-dpdk) is necessary.
- [The OVS kernel module installation](#open-vswitch-kernel-module-installation-for-ovs-dpdk) is necessary.

```
docker compose -f jumpstart-on-docker/compose.yaml/basic/ovs-dpdk/compose.yaml up
```

## [bench-iip](https://github.com/yasukata/bench-iip) on DPDK

- [Dockerfile/bench-iip-dpdk](Dockerfile/bench-iip-dpdk)
- [compose.yaml/basic/bench-iip-dpdk](compose.yaml/basic/bench-iip-dpdk)
- Two containers run the bench-iip application using DPDK: one serves as the server, and the other acts as the client. The two containers are directly connected via the virtio/vhost-user interface.
- [Huge pages configuration](#huge-pages-configuration-for-dpdk) is necessary.

```
docker compose -f jumpstart-on-docker/compose.yaml/basic/bench-iip-dpdk/compose.yaml up
```

## [bench-iip](https://github.com/yasukata/bench-iip) on AF_XDP

- [Dockerfile/bench-iip-af_xdp](Dockerfile/bench-iip-af_xdp)
- [compose.yaml/basic/bench-iip-af_xdp](compose.yaml/basic/bench-iip-af_xdp)
- Two containers run the bench-iip application using AF_XDP: one works as the server, and the other is the client. The two containers use veth interfaces associated with the same Linux bridge.

```
docker compose -f jumpstart-on-docker/compose.yaml/basic/bench-iip-af_xdp/compose.yaml up
```

## [mimicached](https://github.com/yasukata/mimicached) on DPDK

- [Dockerfile/mimicached-iip-dpdk](Dockerfile/mimicached-iip-dpdk)
- [compose.yaml/basic/mimicached-iip-dpdk](compose.yaml/basic/mimicached-iip-dpdk)
- A container runs mimicached using DPDK, and another container sends requests to it using the bench-iip application using DPDK. The two containers are directly connected via the virtio/vhost-user interface.
- [Huge pages configuration](#huge-pages-configuration-for-dpdk) is necessary.

```
docker compose -f jumpstart-on-docker/compose.yaml/basic/mimicached-iip-dpdk/compose.yaml up
```

## [mimicached](https://github.com/yasukata/mimicached) on AF_XDP

- [Dockerfile/mimicached-iip-af_xdp](Dockerfile/mimicached-iip-af_xdp)
- [compose.yaml/basic/mimicached-iip-af_xdp](compose.yaml/basic/mimicached-iip-af_xdp)
- A container runs mimicached using AF_XDP, and another container sends requests to it using the bench-iip application using AF_XDP. The two containers use veth interfaces associated with the same Linux bridge.

```
docker compose -f jumpstart-on-docker/compose.yaml/basic/mimicached-iip-af_xdp/compose.yaml up
```

## OVS-DPDK bridging [bench-iip](https://github.com/yasukata/bench-iip) containers

- [compose.yaml/extra/ovs-dpdk/bench-iip-dpdk](compose.yaml/extra/ovs-dpdk/bench-iip-dpdk)
- Two containers run the bench-iip application using DPDK: one serves as the server and the other acts as the client. The data is forwarded by an OVS-DPDK switch run by another container. The containers are connected via the virtio/vhost-user interfaces.
- [Huge pages configuration](#huge-pages-configuration-for-dpdk) is necessary.
- [The OVS kernel module installation](#open-vswitch-kernel-module-installation-for-ovs-dpdk) is necessary.

```
docker compose -f jumpstart-on-docker/compose.yaml/extra/ovs-dpdk/bench-iip-dpdk/compose.yaml up
```

## OVS-DPDK bridging [mimicached](https://github.com/yasukata/mimicached) and [bench-iip](https://github.com/yasukata/bench-iip) containers

- [compose.yaml/extra/ovs-dpdk/mimicached-iip-dpdk](compose.yaml/extra/ovs-dpdk/mimicached-iip-dpdk)
- A container runs mimicached using DPDK, and another container sends requests to it using the bench-iip application using DPDK. The data is forwarded by an OVS-DPDK switch run by another container. The containers are connected via the virtio/vhost-user interfaces.
- [Huge pages configuration](#huge-pages-configuration-for-dpdk) is necessary.
- [The OVS kernel module installation](#open-vswitch-kernel-module-installation-for-ovs-dpdk) is necessary.

```
docker compose -f jumpstart-on-docker/compose.yaml/extra/ovs-dpdk/mimicached-iip-dpdk/compose.yaml up
```

## An echo server written in Go and using [iip](https://github.com/yasukata/iip) and AF_XDP over cgo

- [compose.yaml/extra/go/echo-iip-af_xdp](compose.yaml/extra/go/echo-iip-af_xdp)
- A container runs an echo server which is written in Go and uses iip and AF_XDP through cgo, and another container sends requests to it using the bench-iip application using AF_XDP. The two containers use veth interfaces associated with the same Linux bridge.

```
docker compose -f jumpstart-on-docker/compose.yaml/extra/go/echo-iip-af_xdp/compose.yaml up
```

