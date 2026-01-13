# Comparison: Docker vs VMs

| Feature            | Docker                       | Virtual Machines                |
|--------------------|-------------------------------|----------------------------------|
| Startup Time       | Seconds                       | Minutes                          |
| Resource Usage     | Low                           | High                             |
| Persistence        | Needs Dockerfile or volume    | Yes                              |
| Networking         | Needs setup (`labnet`)        | Easier with dual NIC             |
| Performance Tools  | `top`, `time`, `docker stats` | `htop`, `perf`, `gnome-monitor`  |

![Ping RTT Comparison](ping_rtt_vm_vs_docker.png)

## Performance
Docker containers started faster and consumed fewer resources due to shared host kernels.

## Isolation & Security
Virtual machines provided stronger isolation, making them more suitable for untrusted workloads.

## Use Cases
Docker is ideal for CI pipelines and testing labs, while VMs suit legacy systems and full OS requirements.

**Conclusion:** Docker is lighter and faster for quick testing. VMs are better for full OS experience.
