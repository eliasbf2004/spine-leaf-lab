# spine-leaf-lab

Hands-on network labs built with [Containerlab](https://containerlab.dev) and
FRRouting (FRR) on WSL2. The goal is to go step by step from a basic LAN to a
spine-leaf data center fabric running OSPF and eBGP, and to measure the results
(convergence time, ECMP paths, manual vs. automated configuration time).

## Roadmap

| Level | Lab | Status |
|-------|-----|--------|
| 1 | Two LANs connected by a router | Done |
| 2 | Three routers in line: static routes, then OSPF | Planned |
| 3 | Router triangle with OSPF and link failure | Planned |
| 4 | Spine-leaf fabric with OSPF, then eBGP and ECMP | Planned |
| 5 | Configuration automation with Python | Planned |

## Labs

- [Level 1: Two LANs connected by a router](nivel1/README.md)