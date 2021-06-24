```mermaid
sequenceDiagram
    scap_bpf_load()->>load_bpf_file(): call
    load_bpf_file()->>load_bpf_file(): struct bpf_map_data maps[BPF_MAP_MAX]
    load_bpf_file()->>load_bpf_file(): osname = uname()
    opt ELF version == nil
        load_bpf_file()-->>load_bpf_file(): error, goto cleanup
    end
    load_bpf_file()->>load_bpf_file(): fd = open(probepath)
    load_bpf_file()->>libelf: elf_begin()
    Note left of libelf: read ELF format
    libelf-->>load_bpf_file(): elf descriptor
    load_bpf_file()->>gelf: gelf_getehdr()
    note left of gelf: read ELF header
    gelf-->>load_bpf_file(): elf header
    loop i := 0, i < shnum, i++
        load_bpf_file()->>libelf: get_elf_section()
        Note left of libelf: get section i-th header, name, data
        Note over load_bpf_file(): store index of maps
        Note over load_bpf_file(): store data and links of symbols
        opt shname == "kernel_version" && shdata != osname.release
            Note right of load_bpf_file(): eBPF probe compiled for another kernel release
            load_bpf_file()-->>load_bpf_file(): error, goto cleanup
        end
        opt shname == "probe_version" && shdata != driver version
            Note right of load_bpf_file(): eBPF probe version does not match the Falco driver version
            load_bpf_file()-->>load_bpf_file(): error, goto cleanup
        end
    end

    load_bpf_file()->>load_elf_maps_section(): call
    load_elf_maps_section()->>libelf: elf_getscn
    load_elf_maps_section()->>libelf: elf_getdata
    Note over load_elf_maps_section(): count number of maps
    Note over load_elf_maps_section(): sort the maps
    loop i := 0, i < numMaps, i++
    Note over load_elf_maps_section(): def = bpf_map_def{}
    Note over load_elf_maps_section(): assign to def the corresponding portion of ELF data
    Note over load_elf_maps_section(): update maps[i] ELF offset
    Note over load_elf_maps_section(): maps[i].def = def
    end
    load_elf_maps_section()-->>load_bpf_file(): error status
    load_bpf_file()->>load_maps(): call
    loop i := 0, i < numMaps, i++
    opt i == PERF_MAP || LOCAL_STATE_MAP || ...
        Note over load_maps(): set the max_entries attribute of current map equal to the number of online CPUs
    end
    load_maps()->>eBPF VM: sys_bpf(BPF_MAP_CREATE, ...)
    eBPF VM-->>load_maps(): file descriptor fd
    Note over load_maps(): store fd if valid
    opt maps[i].type == MAP_TYPE_PROG_ARRAY
        Note over load_maps(): store the index i as the position where this eBPF map is
    end
    end
    load_maps()-->>load_bpf_file(): error status


    loop i := 0, i < shnum, i++
        Note over load_bpf_file(): get section i-th header, name, data
        opt shtype == SHT_REL
        load_bpf_file()->>parse_relocations(): call
        parse_reloactions()-->>load_bpf_file(): error status
        end
    end
    loop i := 0, i < shnum, i++
        Note over load_bpf_file(): get section i-th header, name, data
        opt shname starts with "[raw_]tracepoint/"
        load_bpf_file()->>load_tracepoint(): call
        load_tracepoint()-->>load_bpf_file(): error status
        end
    end
    opt cleanup or error
    load_bpf_file()->>libelf: elf_end()
    load_bpf_file()->>load_bpf_file(): close(fd)
    load_bpf_file()-->>scap_bpf_load(): status
    end
    Note over scap_bpf_load(): ...
```

TODO: sysdig_map_types
