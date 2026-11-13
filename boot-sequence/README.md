# Boot Sequence

What PCs, servers and virtual machines do between pressing the power button and being ready: BIOS vs UEFI, Windows vs Linux vs macOS, boot order rules, disk layouts and timings.

Open `index.html` in a browser, or view it live: https://build-things-that-break-things.github.io/from-enter-to-pixels/boot-sequence/

## How it's organised

Every system follows the same **seven-stage relay**:

1. Power on
2. Firmware start
3. Choose a boot device
4. Boot loader
5. Kernel start
6. First process and services
7. Ready

Stages 1–3 are the same whatever OS you run; from stage 4 each OS does things its own way.

Systems are compared in **four groups**:

| Group | Compares | Stages |
|---|---|---|
| Firmware | Legacy BIOS · UEFI · coreboot & others | 1–4 |
| Operating systems | Windows · Linux · macOS (Apple silicon) | 4–7 |
| Machine types | Desktop/laptop · Server · VM/cloud | 1–7 |
| Network boot | Classic PXE · iPXE · UEFI HTTP Boot · iSCSI / SAN boot | 3–6 |

Stage colours (`--s1`…`--s7`) are the same in every view.

## Views

Add `#overview`, `#side`, `#sim`, `#disk`, `#timeline`, `#compare` or `#deep` to the URL to open a view directly.

- **High level:** the seven stages as an animated journey, with plain-language explanations and everyday comparisons. It also has a **"Where did it stop?" troubleshooter**: pick a symptom (beeps, "No bootable device", `PXE-E53`, `grub rescue>`, INACCESSIBLE_BOOT_DEVICE…) to see which stage failed.
- **Interview** group:
  - **Side by side:** every system in the group going through the same stage, as animated sequence diagrams.
  - **Boot order:** a simulator. Set the firmware mode, Secure Boot, the disk type, the NVRAM entry, a USB stick, a one-time choice and network boot, and step through the firmware's decision. Seven ready-made scenarios are the steps.
  - **Disk layout:** the MBR sector byte by byte, the GPT layout, and the files inside the EFI System Partition, plus `efibootmgr` output.
  - **Timeline:** typical seconds per stage as a waterfall, either fitted per bar or on a shared scale.
  - **Compare:** a table per group, plus a rule of thumb.
- **Deep internals:** 15 topics, as animated system diagrams or sequence diagrams. The side panel shows the stage, an "in one sentence" summary and which systems each topic applies to.
  - *Firmware:* CPU modes from real to long mode · UEFI phases (SEC, PEI, DXE, BDS) · the Secure Boot key hierarchy (PK, KEK, db, dbx, shim, MOK, BlackLotus).
  - *Operating systems:* measured boot and automatic disk unlock (TPM PCRs) · starting the other CPU cores (INIT-SIPI-SIPI) · initramfs to switch_root · Windows boot drivers and ELAM · systemd's dependency graph.
  - *Network boot:* network boot packet by packet (DHCP options, TFTP, iPXE chain-loading, HTTPS) · diskless root over NFS · booting from an iSCSI disk (iBFT handover) · network boot security (rogue DHCP, DHCP snooping, HTTPS, Secure Boot).
  - *Machine types:* the BMC · booting without firmware (Firecracker microVMs) · sleep, hibernate and Fast Startup.

## Controls

Same as the other projects: group picker, arrow keys (steps, then the next group), Pause animation (space bar), and a looping Auto-advance.

## Structure of `index.html`

- `STAGES`: the seven stages, each with a `plain` block for High level.
- `ITEMS`: one entry per system:
  - `actors`: the lanes
  - `ph[stageId]`: `s`, the sequence (`[from, to, label]`, or `[lane, "=", note]` for a note), and `t`, the text
- `GROUPS`: the systems in each group, which stages apply, a per-stage `diff`, and a `verdict`.
- `COMPARE` and `CMP`: compare-table rows and values. `TIMES`: illustrative seconds per stage.
- `TROUBLE`: troubleshooter symptoms. `PRESETS` and `simulate()`: the boot order simulator. `DISK`: disk layouts and the EFI partition file tree.
- `DEEPT` and `DGRAPH`: the deep topics and their diagrams.
- Views: `HighView`, `SideView`, `SimView`, `DiskView`, `TimeView`, `CompareView` and `DeepView`. Shared helpers (`Flow`, `seqDiagram`, `drawGraph`, `cycler`) are copied from the other projects.

The simulator's rules:
- **UEFI:** one-time choice (F12 / BootNext) → BootOrder entries, with a Secure Boot veto → fallback `\EFI\BOOT\BOOTX64.EFI` → "No bootable device".
- **Legacy:** boot menu → CMOS order, where the first sector ending in `0x55AA` wins.

## Adding a deep topic

Add an entry to `DEEPT[group]` with `title`, `stage` (which boot stage it belongs to, which sets the colour), `items` (the systems it applies to), `sum`, `text`, `facts`, `say` and a `kind`:
- `"graph"`: `g` (a `DGRAPH` id) and `m` (messages routed along the links)
- `"seq"`: `actors` and `rows`, in the same format as the stages

`DeepView` picks it up automatically.
