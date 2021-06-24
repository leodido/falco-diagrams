```mermaid
sequenceDiagram
    Note over scap_open(): case SCAP_MODE_LIVE
    alt userspace
        scap_open()->>scap_open_udig_int(): call
        scap_open_udig_int()-->>scap_open(): scap_t
    else
        scap_open()->>scap_open_live_int(): call
        scap_open_live_int()->>scap_open_live_int(): bpf_probe = getenv("FALCO_BPF_PROBE")
        alt FALCO_BPF_PROBE set
            Note over scap_open_live_int(): bpf = true
            opt FALCO_BPF_PROBE empty
            scap_open_live_int()->>scap_open_live_int(): bpf_probe = $HOME/.falco/falco-bpf.o
            Note over scap_open_live_int(): fallback to standard path
            end
        else
            Note over scap_open_live_int(): bpf = false
        end
        Note over scap_open_live_int(): ...
    end
    scap_open_live_int()-->>scap_open(): scap_t
```
