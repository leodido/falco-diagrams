```mermaid
sequenceDiagram
scap_bpf_load()->>scap_bpf_load(): nOnline = 0
loop i := range nCPUs
scap_bpf_load()->>scap_bpf_load(): attr = new perf_event_attr
opt i > 0
scap_bpf_load()->>scap_bpf_load(): online = /sys/devices/system/cpu/cpu<i>/online
opt ! online
Note over scap_bpf_load(): jump to the next CPU
end
end
opt nOnline++ > nDevices
Note over scap_bpf_load(): set error status and return it
end
scap_bpf_load()->>kernel: syscall(__NR_perf_event_open, attr , pid = -1, cpu = <i>, group_fd = -1, flags = 0)
kernel-->>scap_bpf_load(): pmu_fd
scap_bpf_load()->>kernel: syscall(BPF_MAP_UPDATE, ...)
Note right of scap_bpf_load(): store pmu_fd at the i-th position of the SYSDIG_PERF_MAP map
kernel-->>scap_bpf_load(): error status
scap_bpf_load()->>kernel: ioctl(pmu_fd, PERF_EVENT_IOC_ENABLE, 0)
kernel-->>scap_bpf_load(): error status
scap_bpf_load()->>perf_event_mmap(): map the ring buffer
perf_event_mmap()-->>scap_bpf_load(): error status
scap_bpf_load()->>scap_bpf_load(): nOnline++
end
opt nOnline != nDevices
Note over scap_bpf_load(): set error status and return it
end
Note over scap_bpf_load(): ...
```

TODO:

- Show `perf_event_attr`
- Sho `perf_event_open` (set up performance monitoring for one event, read man)
  - pid -1 & cpu >= 0 measures all processes/threads on the specified CPU (`CAP_SYS_ADMIN` capability required)
- `ioctl` to enable events
