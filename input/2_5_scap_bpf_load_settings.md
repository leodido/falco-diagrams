```mermaid
sequenceDiagram
scap_bpf_load()->>set_default_settings(): call
set_default_settings()->>set_boot_time(): call
set_boot_time()-->>set_default_settings(): t = boot time
set_default_settings()->>set_default_settings(): s = sysdig_bpf_settings{ t,...}
set_default_settings()->>kernel: syscall(BPF_MAP_UPDATE, ...)
Note left of set_default_settings(): store the settings into the SYSDIG_SETTINGS_MAP map at the first (0) position
kernel-->>set_default_settings(): error status
set_default_settings()-->>scap_bpf_load(): error status
```
