# spine-leaf-lab
# Level 1: Two LANs connected by a router

A first lab built with [Containerlab](https://containerlab.dev) and FRRouting (FRR) on WSL2.
Two hosts in different subnets communicate through a single router.

## Topology

```
PC1 ---- R1 ---- PC2
```

| Device | Interface | IP address        |
|--------|-----------|-------------------|
| PC1    | eth1      | 192.168.1.10/24   |
| R1     | eth1      | 192.168.1.1/24    |
| R1     | eth2      | 192.168.2.1/24    |
| PC2    | eth1      | 192.168.2.10/24   |

- R1 runs `quay.io/frrouting/frr:8.5.4`
- PC1 and PC2 run `alpine:latest`

## Deploy

```
sudo containerlab deploy -t lab1.clab.yml
```

## Configuration

**PC1**
```
docker exec -it clab-lab1-pc1 sh
ip addr add 192.168.1.10/24 dev eth1
ip route replace default via 192.168.1.1
```

**PC2**
```
docker exec -it clab-lab1-pc2 sh
ip addr add 192.168.2.10/24 dev eth1
ip route replace default via 192.168.2.1
```

**R1**
```
docker exec -it clab-lab1-r1 sh
ip addr add 192.168.1.1/24 dev eth1
ip addr add 192.168.2.1/24 dev eth2
exit
```

Check the result in FRR's shell:
```
docker exec -it clab-lab1-r1 vtysh
show interface brief
```

## Verification

From PC1:
```
ping -c 3 192.168.1.1     # gateway
ping -c 3 192.168.2.10    # PC2, through R1
```

## Lessons learned

- Containerlab gives every container a default route through `eth0` (management network).
  `ip route add default` fails with "File exists", so the route has to be replaced with
  `ip route replace default via <gateway>` to send traffic through R1.
- The alpine containers exit immediately unless given a long-running command
  (`cmd: sleep infinity` in the topology file).
- Docker Desktop's engine runs in a separate VM, so Containerlab cannot reach the
  containers' network namespaces from WSL. Docker Engine installed directly inside Ubuntu fixes it.
- Configuration applied by hand is not persistent: redeploying the lab removes it.
