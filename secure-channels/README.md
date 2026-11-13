# Secure Channels

How HTTPS, FTPS, SFTP, SCP, SSH tunnels, IPsec, OpenVPN, WireGuard and Tailscale work, and how they compare.

Open `index.html` in a browser, or view it live: https://build-things-that-break-things.github.io/from-enter-to-pixels/secure-channels/

## How it's organised

These channels don't all do the same job, so they're compared within **three groups**:

| Group | Protocols | Question |
|---|---|---|
| File transfer | FTPS, SFTP, SCP | What's the secure way to move files? |
| App & port | HTTPS, SSH tunnel | Protect one app, or carry one port through a pipe? |
| Whole network | IPsec, OpenVPN, WireGuard, Tailscale | How should whole devices and networks connect privately? |

Every protocol is described in the same **six phases**, so any two can be compared step by step:

1. Reach
2. Agree on keys
3. Prove identity
4. Carry data
5. What leaks
6. Keep alive

Colours encode the **engine** underneath: TLS, SSH, IPsec or WireGuard.

## Views

The tabs run left to right from simple to detailed. Add `#overview`, `#side`, `#layers`, `#topology`, `#compare` or `#deep` to the URL to open a view directly.

- **High level:** three animated scenes (protect one app / one path / the whole network), the family of protocols with plain-language explanations, and a "Which one should I use?" question path.
- **Interview** group:
  - **Side by side:** all protocols in the group going through the same phase, as animated sequence diagrams.
  - **Layer stack:** one packet per protocol, built from the inside out, with the encrypted part outlined.
  - **Topology:** the connection shape for each protocol: direct, through a bastion, hub and spoke, point to point, or mesh.
  - **Compare:** a table of the group against the same questions, plus a rule of thumb.
- **Deep internals:** 13 topics across the three groups. Each uses one of three visuals: a clickable byte layout, an animated system diagram, or a sequence diagram.
  - *File transfer:* the SSH binary packet, an SFTP request byte by byte, FTPS and the firewall problem, inside the SSH server (privilege separation).
  - *App & port:* the TLS 1.3 key schedule, TLS on the wire (record and ClientHello), inside a port forward (channels, windows, TCP over TCP), two trust models (CAs vs known_hosts and SSH CAs).
  - *Whole network:* kernel VPNs (WireGuard, IPsec xfrm), user-space VPNs (OpenVPN, wireguard-go), ESP on the wire, WireGuard messages and Noise IK, Tailscale NAT traversal.

## Controls

- **Group picker:** File transfer · App & port · Whole network.
- **← / →** (or the arrow keys): through the view's steps (phases, protocols or scopes), then on to the next group.
- **Pause animation** (or the space bar): freezes every animation.
- **Auto-advance:** steps on automatically and loops through all groups.

## Structure of `index.html`

- `ENGINES`: TLS, SSH, IPsec and WireGuard, with colour, trust model and users.
- `P`: one entry per protocol:
  - `actors`: the lanes in the sequence diagrams
  - `ph`: the six phases, each with `s`, the sequence (`[from, to, label]`, or `[lane, "=", note]` for a note), and `t`, the text. The `eaves` phase has `vis`/`hid` lists instead.
  - `cmp`: the compare-table values
  - `packets`: the layer-stack bars, as blocks `[kind, label, detail, encrypted]`
  - `topo`: the topology, with graph id, label overrides and messages
  - `plain`: the high-level text
- `GROUPS`: the protocols in each group, plus a per-phase `diff`, `layerNote` and `verdict`.
- `PHASES`, `SCOPES`, `DT` (the decision path), and `TOPO` (the topology graphs).
- `DEEPT` (deep topics per group) and `DGRAPH` (their system diagrams).
- Views: `HighView`, `SideView`, `LayerView`, `TopoView`, `CompareView` and `DeepView`. Each has `steps()`, `render(host)`, `notes(el)` and `stop()`. Renderers: `seqDiagram()`, `bytesView()`, and `drawGraph()` (with bands for user space / kernel / hardware). The `Flow` animation engine and `drawGraph()` are copied from web-page-load, so this folder stays self-contained.

## Adding a deep topic

Add an entry to `DEEPT[group]` with `title`, `sum`, `text`, `facts`, `say`, `protos` and a `kind`:
- `"bytes"`: `layouts: [{lbl, f: [[name, size, "clear"|"enc"|"tag"|"mix", description]]}]`
- `"graph"`: `g` (a `DGRAPH` id) and `m` (messages), with optional `notes`
- `"seq"`: `actors` and `rows`, in the same format as the phases

`DeepView` picks it up automatically. The byte layout highlights one field at a time and respects Pause animation; any field can be clicked.
