# PromQL Queries

## CPU Usage

```promql
100 - (avg by(instance)(rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
```

## Memory %

```promql
100*(1-node_memory_MemAvailable_bytes/node_memory_MemTotal_bytes)
```

## Disk %

```promql
100-(node_filesystem_avail_bytes{mountpoint="/"}*100/node_filesystem_size_bytes{mountpoint="/"})
```

## Network Receive

```promql
rate(node_network_receive_bytes_total[5m])
```

## Network Transmit

```promql
rate(node_network_transmit_bytes_total[5m])
```
