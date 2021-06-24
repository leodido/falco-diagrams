```mermaid
sequenceDiagram
    scap_bpf_load()->>set_runtime_params(): call
    set_runtime_params()->>kernel: setrlimit(INFINITY)
    kernel-->>set_runtime_params(): error status
    set_runtime_params()->>kernel: /proc/sys/net/core/bpf_jit_enable = 1
    kernel-->>set_runtime_params(): error status
    set_runtime_params()->>kernel: /proc/sys/net/core/bpf_jit_harden = 0
    kernel-->>set_runtime_params(): error status
    set_runtime_params()->>kernel: /proc/sys/net/core/bpf_jit_kallsyms = 1
    kernel-->>set_runtime_params(): error status
    set_runtime_params()-->>scap_bpf_load(): error status
    Note over scap_bpf_load(): ...
```
