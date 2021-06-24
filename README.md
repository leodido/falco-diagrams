# falco-diagrams

> Diagrams to **visually** explain [Falco](https://github.com/falcosecurity/falco)

Diagrams are done with [Mermaid](https://mermaid-js.github.io).

Since I believe a picture is worth a thousand words, even more when trying to explain complicated stuff as Falco and its eBPF driver are... Here we are!

This repo also provides tooling for automating the making of these diagrams:

a GiHub action to automatically **generate, commit, and push Falco diagrams** to the `output/` directory.

But since I didn't like so much the quality of the resulting diagram images, I created a **simple** in-place mermaid **editor** that you can use to:

1. **draw** diagrams with a **custom Falco theme**
2. **download** them as **PNG** images with a good resolution

I've already created some of them for my [talks](https://youtube.com/playlist?list=PL-YnLgW35W60wX9lhu6-a8ln4I9yfT1ug) and [decks](https://github.com/leodido/presentations) in the past, so you can find them in the [`themed/`](./themed) directory. Enjoy them!

**Disclaimer**: it's possible that you'll find some of these diagrams and illustrations (and many others) in the upcoming [Falco Cookbook](https://gum.co/falco). Grab it until it's hot!

<table style="padding:10px">
<tr>
<td><img src="themed/0_falco_starts_sinsp_scap.png" alt="falco starts sinsp scap"/></td>
<td><img src="themed/1_0_scap_open.png" alt="scap open"/></td>
<td><img src="themed/1_1_scap_open_internals.png" alt="scap open internals"/></td>
</tr>
<tr>
<td><img src="themed/1_2_scap_open_live_int.png" alt="scap open live int"/></td>
<td><img src="themed/2_0_scap_bpf_load_initial_setup.png" alt="scap bpf load initial setup"/></td>
<td><img src="themed/2_1_scap_bpf_load_probe_elf_loading.png" alt="scap bpf load probe elf loading"/></td>
</tr>
<tr>
<td><img src="themed/2_2_scap_bpf_load_populate_routing_syscall_table.png" alt="scap bpf load populate routing syscall table"/></td>
<td><img src="themed/2_3_scap_bpf_load_populate_event_table_and_fillers_table.png" alt="scap bpf load populate event table and fillers table"/></td>
<td><img src="themed/2_4_scap_bpf_load_events_per_cpu.png" alt="scap bpf load events per cpu"/></td>
</tr>
<tr>
<td><img src="themed/2_5_scap_bpf_load_settings.png" alt="scap bpf load settings"/></td>
<td><img src="themed/3_0_falco_event_loop.png" alt="falco event loop"/></td>
<td><img src="themed/3_1_falco_event_loop_with_queue.png" alt="falco event loop with queue"/></td>
</tr>
<tr>
<td><img src="themed/4_0_falco_grpc_server_on.png" alt="falco grpc server on"/></td>
<td><img src="themed/4_1_falco_grpc_server_spawning_threads.png" alt="falco grpc server spawning threads"/></td>
<td><img src="themed/4_2_falco_grpc_server_thread_processing.png" alt="falco grpc server thread processing"/></td>
</tr>
<tr>
<td><img src="themed/4_3_falco_grpc_server_bidirectional_handling.png" alt="falco grpc server bidirectional handling"/></td>
<td><img src="themed/4_4_falco_grpc_server_stream_handling.png" alt="falco grpc server stream handling"/></td>
<td><img src="themed/5_0_falco_poiana_bot_workflow.png" alt="falco poiana bot workflow"/></td>
</tr>
</table>

## Contribute a diagram

It's simple!

Just create a `.md` file containing some Mermaid syntax in the `input/` directory. Something like:

```mermaid
sequenceDiagram
  autonumber
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

This repo's tooling will do the rest for you and you'll obtain in a minute the resulting diagram in the `output/` directory.

Notice that also the input file will be edited by the tooling by inserting into it the diagram image just generated.

## TODOs

- [ ] Make the in-place editor work also on other browsers other than Google Chrome
- [ ] Create a real in-place editor with a text-area or similar
  - At the moment, you need to copy the diagram source in the HTML and re-open it
- [ ] Publish the editor
- [ ] Fix the GitHub action compiling the Mermaid diagrams
  - Sizing of the resulting image

---

[![Analytics](https://ga-beacon.appspot.com/UA-49657176-1/falco-diagrams?flat)](https://github.com/igrigorik/ga-beacon)
