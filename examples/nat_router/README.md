# Router and NAT

In this example we use a router with a NAT.
In particular we have:
- 2 switches
- a router connected to a NAT

*configuration:*
  * ns1 (ip 10.0.1.1/24 dg 10.0.1.254)
  * ns2 (ip 10.0.2.1/24 dg 10.0.2.254)
  * ns3 (ip 10.10.1.1/24 dg 10.10.1.100)

*topology*
```bash
ns1 <--> Switch1
            |
            |
      (10.0.1.254/24)
            |
          Router <--> Nat (10.10.1.100) <--> ns3
            |
      (10.0.2.254/24)
            |
            |
ns2 <--> Switch2
```

## Preparing the network namespaces

Execute the [setup.sh](./setup.sh) script:

```bash
sudo ./setup.sh
```

## Launching hover

Before deploying the router, it is necessary to launch the hover daemon.
Please note to kill other instances of hover previously running.

```bash
export GOPATH=$HOME/go
sudo $GOPATH/bin/hoverd -listen 127.0.0.1:5002
```

## Deploying the topology

The [nat_router.yaml](./nat_router.yaml) file contains the configuration script.
To launch the example please execute:

```bash
export GOPATH=$HOME/go
cd $GOPATH/src/github.com/iovisor/iovisor-ovn/examples/switch
$GOPATH/bin/iovisorovnd -file nat_router.yaml -hover http://127.0.0.1:5002
```

## Testing connectivity

In order to allow the arp tables to be correctly completed, please run [arp.sh](./arp.sh) script:

```bash
sudo ./arp.sh
```

Now you are able to test the connectivity TCP and UDP using netcat (nc). Example:

```bash
# Launch netcat server in ns3
sudo ip netns exec ns3 nc -l 8000
# Launch netcat client in ns1
sudo ip netns exec ns1 nc 10.10.1.1 8000
# Launch netcat client in ns2
sudo ip netns exec ns2 nc 10.10.1.1 8000
```

## Debugging

In order to see the debug output generated by the IOModule we suggest to see the result of the print on the trace_pipe.

```bash
sudo su
cd /sys/kernel/debug/tracing/
cat trace_pipe
```
