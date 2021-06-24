```mermaid
sequenceDiagram
    scap_bpf_load()->>populate_syscall_routing_table_map(): call
    loop i := 0, i < SYSCALL_TABLE_SIZE, i++
    populate_syscall_routing_table_map()->>syscall_table: g_syscall_code_routing_table[i]
    syscall_table-->>populate_syscall_routing_table_map(): x = system-independent syscall code
    populate_syscall_routing_table_map()->>eBPF VM: sys_bpf(BPF_MAP_UPDATE_ELEM, ...)
    Note right of populate_syscall_routing_table_map(): store x at the i-th position of the SYSDIG_SYSCALL_CODE_ROUTING_TABLE map
    eBPF VM-->>populate_syscall_routing_table_map(): error status
    end
    populate_syscall_routing_table_map()-->>scap_bpf_load(): error status
    scap_bpf_load()->>populate_syscall_table_map(): call
    loop i := 0, i < SYSCALL_TABLE_SIZE, i++
    populate_syscall_table_map()->>syscall_table: g_syscall_table[i]
    Note right of syscall_table: eg., x = {UF_USED | UF_NEVER_DROP, PPME_SYSCALL_OPEN_E, PPME_SYSCALL_OPEN_X}
    syscall_table-->>populate_syscall_table_map(): x = {syscall_flags values + ..., ppm_event_type value}
    populate_syscall_table_map()->>eBPF VM: sys_bpf(BPF_MAP_UPDATE_ELEM, ...)
    Note left of populate_syscall_table_map(): store x at the i-th position of the SYSDIG_SYSCALL_TABLE map
    eBPF VM-->>populate_syscall_table_map(): error status
    end
    populate_syscall_table_map()-->>scap_bpf_load(): error status
    Note over scap_bpf_load(): ...
```

TODO:

- What type of eBPF maps SYSDIG_SYSCALL_CODE_ROUTING_TABLE and SYSDIG_SYSCALL_TABLE are?
- Show syscall_flags enum
- Show ppm_event_type enum (start, exit)
