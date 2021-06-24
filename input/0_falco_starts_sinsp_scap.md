<!-- generated by mermaid compile action - START -->
![~mermaid diagram 1~](/output/input_0-falco_starts_sinsp_scap-md-1.png)
<details>
  <summary>Mermaid markup</summary>

```mermaid
sequenceDiagram
    falco->>sinsp: new()
    sinsp-->>falco: inspector
    falco->>sinsp: open()
    Note over sinsp: open_live_common(..., SCAP_MODE_LIVE)
    sinsp->>scap: scap_open()
    Note over scap: REF: SCAP OPEN
    scap-->>sinsp: scap_t
    sinsp->>sinsp: init
    sinsp-->>falco: void
```

</details>
<!-- generated by mermaid compile action - END -->