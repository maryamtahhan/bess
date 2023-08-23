## BESS (Berkeley Extensible Software Switch)

BESS (formerly known as [SoftNIC](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2015/EECS-2015-155.html)) is a modular framework for software switches. BESS itself is *not* a virtual switch; it is neither pre-configured nor hard-coded to provide particular functionality, such as Ethernet bridging or OpenFlow-driven switching. Instead, you (or an external controller) can *configure* your own packet processing datapath by composing small "modules". While the basic concept is similar to [Click](http://read.cs.ucla.edu/click/click), BESS does not sacrifice performance for programmability.

BESS was created by Sangjin Han and is developed at the University of California, Berkeley and at Nefeli Networks. [Contributors to BESS](https://github.com/omec-project/bess/blob/master/CONTRIBUTING.md) include students, researchers, and developers who care about networking with high performance and high customizability. BESS is open-source under a BSD license.

If you are new to BESS, we recommend you start here:

1. [BESS Overview](https://github.com/omec-project/bess/wiki/BESS-Overview)
2. [Build and Install BESS](https://github.com/omec-project/bess/wiki/Build-and-Install-BESS)
3. [Write a BESS Configuration Script](https://github.com/omec-project/bess/wiki/Writing-a-BESS-Configuration-Script)
4. [Connect BESS to a Network Interface, VM, or Container](https://github.com/omec-project/bess/wiki/Hooking-up-BESS-Ports)

To install BESS on Linux quickly, you can download the binary from [Release](https://github.com/omec-project/bess/releases/latest). Please refer to [GCC x86 Options](https://gcc.gnu.org/onlinedocs/gcc/x86-Options.html) to determine which tarball to use. Suppose `bess-core2-linux.tar.gz` is downloaded:

    sudo apt-get install -y python python-pip libgraph-easy-perl
    pip install --user protobuf grpcio scapy
    sudo sysctl vm.nr_hugepages=1024  # For single NUMA node systems
    tar -xf bess-core2-linux.tar.gz
    cd bess/
    make -C core/kmod # Build the kernel module (optional)
    bessctl/bessctl

Documentation can be found [here](https://github.com/omec-project/bess/wiki/). Please consider [contributing](https://github.com/omec-project/bess/wiki/How-to-Contribute) to the project!!


## DPDK 23.03 build

Start a docker container

```bash
docker run -ti -v /$PATH_TO_BESS/bess:/bess --cap-add=ALL -v /mnt/huge:/mnt/huge -v /sys/bus/pci/devices:/sys/bus/pci/devices -v /sys/devices/system/node:/sys/devices/system/node -v /lib/modules:/lib/modules -v /dev:/dev  ubuntu:22.04
```

Install dependencies

```bash
apt update && apt install -y make apt-transport-https ca-certificates g++ make pkg-config libunwind8-dev liblzma-dev zlib1g-dev libpcap-dev libssl-dev libnuma-dev git libgflags-dev libgoogle-glog-dev libgraph-easy-perl libgtest-dev libgrpc++-dev libprotobuf-dev libc-ares-dev libbenchmark-dev libgtest-dev protobuf-compiler-grpc python3-scapy python3-pip python3 meson python3-pyelftools curl build-essential libbsd-dev libelf-dev libjson-c-dev libnl-3-dev libnl-cli-3-dev libnuma-dev libpcap-dev meson pkg-config libbpf-dev gcc-multilib clang llvm lld m4 vim wget

pip install --user protobuf grpcio scapy pyelftools meson

export PROTOCOL_BUFFERS_PYTHON_IMPLEMENTATION=python

echo 1024 > /sys/kernel/mm/hugepages/hugepages-2048kB/nr_hugepages
```

Build bess

```bash
./build.py -v bess

$ cd bessctl/
$ ./bessctl
Type "help" for more information.
Connection to localhost:10514 failed
Perhaps bessd daemon is not running locally? Try "daemon start".
<disconnected> $
> daemon start
> run samples/acl
> monitor pipeline
```
