# Memory tuning

## Overcommit

### Inspect

```bash
sysctl vm.overcommit_memory

grep -E 'CommitLimit|Committed_AS|MemTotal|SwapTotal' /proc/meminfo
```

### Configure

```bash
# Test / Dev Environments
sysctl -w vm.overcommit_memory=1

# DB / Kafka
sysctl -w vm.overcommit_memory=2
sysctl -w vm.overcommit_ratio=80

# Stateless
sysctl -w vm.overcommit_memory=0
```

## Swap and dirty pages

### Inspect

```bash
sysctl vm.swappiness
sysctl vm.dirty_background_ratio
sysctl vm.dirty_ratio
```

### Configure

```bash
sysctl -w vm.swappiness=1
sysctl -w vm.dirty_background_ratio=5
sysctl -w vm.dirty_ratio=10
```

## Transparent Huge Pages (THP)

### Inspect

```bash
cat /sys/kernel/mm/transparent_hugepage/enabled
cat /sys/kernel/mm/transparent_hugepage/defrag
```

### Configure

```bash
echo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled
echo never | sudo tee /sys/kernel/mm/transparent_hugepage/defrag
echo 0     | sudo tee /sys/kernel/mm/transparent_hugepage/khugepaged/defrag
```
