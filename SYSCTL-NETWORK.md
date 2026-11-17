# Network tuning

## Local port range

### Inspect

```bash
sysctl net.ipv4.ip_local_port_range
```

### Configure

```bash
sysctl -w net.ipv4.ip_local_port_range="1024 65535"
```

## TCP behavior and connection tracking

### Inspect

```bash
sysctl net.ipv4.tcp_fin_timeout
sysctl net.ipv4.tcp_tw_reuse
sysctl net.ipv4.tcp_max_tw_buckets
sysctl net.ipv4.tcp_timestamps
sysctl net.ipv4.tcp_sack
sysctl net.ipv4.tcp_window_scaling
sysctl net.ipv4.tcp_syn_retries
sysctl net.ipv4.tcp_synack_retries
sysctl net.ipv4.tcp_retries2
sysctl net.ipv4.tcp_keepalive_time
sysctl net.ipv4.tcp_keepalive_intvl
sysctl net.ipv4.tcp_keepalive_probes
sysctl net.ipv4.tcp_congestion_control
sysctl net.netfilter.nf_conntrack_max
```

### Configure

```bash
sysctl -w net.ipv4.tcp_fin_timeout=5
sysctl -w net.ipv4.tcp_tw_reuse=1
sysctl -w net.ipv4.tcp_max_tw_buckets=500000
sysctl -w net.ipv4.tcp_timestamps=1
sysctl -w net.ipv4.tcp_sack=1
sysctl -w net.ipv4.tcp_window_scaling=1
sysctl -w net.ipv4.tcp_syn_retries=5
sysctl -w net.ipv4.tcp_synack_retries=3
sysctl -w net.ipv4.tcp_retries2=8
sysctl -w net.ipv4.tcp_keepalive_time=300
sysctl -w net.ipv4.tcp_keepalive_intvl=60
sysctl -w net.ipv4.tcp_keepalive_probes=5
sysctl -w net.ipv4.tcp_congestion_control=cubic
sysctl -w net.netfilter.nf_conntrack_max=262144
```

### Higher TIME_WAIT limit

```bash
sysctl -w net.ipv4.tcp_max_tw_buckets=2000000
```

## High PPS and packet processing

### Inspect

```bash
sysctl net.core.dev_weight
sysctl net.core.dev_weight_rx_bias
sysctl net.core.netdev_budget
sysctl net.core.netdev_budget_usecs
sysctl net.core.busy_read
sysctl net.core.busy_poll
```

### Configure

```bash
sysctl -w net.core.dev_weight=128
sysctl -w net.core.dev_weight_rx_bias=32
sysctl -w net.core.netdev_budget=600
sysctl -w net.core.netdev_budget_usecs=8000
sysctl -w net.core.busy_read=50
sysctl -w net.core.busy_poll=50
```

### Higher packet processing weights

```bash
sysctl -w net.core.dev_weight=300
sysctl -w net.core.dev_weight_rx_bias=64
```

`netdev_budget` and `netdev_budget_usecs` trade CPU time for backlog drainage under bursty traffic.
`busy_read` and `busy_poll` can reduce latency at the cost of extra CPU burn while sockets are polled.

## Socket and device limits

### Inspect

```bash
sysctl vm.max_map_count
sysctl net.core.somaxconn
sysctl net.ipv4.tcp_max_syn_backlog
sysctl net.core.optmem_max
sysctl net.core.netdev_max_backlog
```

### Configure

```bash
sysctl -w vm.max_map_count=1048576
sysctl -w net.core.somaxconn=65535
sysctl -w net.ipv4.tcp_max_syn_backlog=65535
sysctl -w net.core.optmem_max=65536
sysctl -w net.core.netdev_max_backlog=250000
```

## Socket memory

### Inspect

```bash
sysctl net.core.rmem_max
sysctl net.core.wmem_max
sysctl net.core.rmem_default
sysctl net.core.wmem_default
sysctl net.ipv4.tcp_rmem
sysctl net.ipv4.tcp_wmem

sysctl net.ipv4.udp_rmem_min
sysctl net.ipv4.udp_wmem_min
sysctl net.ipv4.udp_mem
```

### Configure

```bash
sysctl -w net.core.rmem_max=16777216
sysctl -w net.core.wmem_max=16777216
sysctl -w net.core.rmem_default=524288
sysctl -w net.core.wmem_default=524288
sysctl -w net.ipv4.tcp_rmem="4096 524288 16777216"
sysctl -w net.ipv4.tcp_wmem="4096 524288 16777216"

sysctl -w net.ipv4.udp_rmem_min=16384
sysctl -w net.ipv4.udp_wmem_min=16384
sysctl -w net.ipv4.udp_mem="762282 1016380 1524564"
```
