# IOVisor-OVN Installation Guide with DevStack

This guide presents the steps to install a testing deployment with OpenStack, OVN and IOVisor-OVN.
If you want to test IOVisor-OVN without OpenStack please see  [README_STANDALONE](./README_STANDALONE.md).

## Installation
In order to create a test environment with a single compute node you should perform the following steps.

1. Install a test system. We recommend to use Ubuntu 16.04, which represents our main developed platform. Please consider that DevStack performs deep changes on the system, so the system should only be dedicated to this purpose.

2. Install DevStack with IOVisor-OVN as network provider, typing the following commands:

```
    git clone http://git.openstack.org/openstack-dev/devstack.git --branch stable/newton
    git clone https://github.com/netgroup-polito/networking-ovn.git --branch stable/newton
    cd devstack
    cp ../networking-ovn/devstack/local.conf.sample local.conf
    ./stack.sh
```
When devstack finishes it shows something like:

```
This is your host ip: 192.168.122.8
Horizon is now available at http://192.168.122.8/
Keystone is serving at http://192.168.122.8:5000/
The default users are: admin and demo
The password: password
2016-11-24 19:34:06.116 | stack.sh completed in 1334 seconds.
```

At this point, your new OpenStack newton instance will use IOVisor-OVN as network provider.

Currently L2 and L3 networks are supported, the IP addresses in the VMs must be configured manually (the DHCP service has not been integrated yet).
