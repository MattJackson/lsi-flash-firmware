# lsi-flash-firmware

Firmware mirror for the [lsi-flash](https://github.com/MattJackson/lsi-flash) tool.
160 unique firmware blobs covering LSI SAS2008 silicon — every public LSI Phase
release (P4 → P20.00.07.00 across IT/IR/BIOS/UEFI personalities) plus
comprehensive Dell PERC H200/H310 family OEM variants, Intel/IBM/Fujitsu OEM
firmware, and SBR templates.

## What this is

A community-maintained, sha256-verified mirror of LSI SAS2008-era firmware
files. Every file in this repo has a manifest entry with provenance + sha256
hash. The `lsi-flash` CLI consumes this manifest to safely cross-flash
PERC H200, PERC H310, IBM M1015, Fujitsu D2607, LSI 9211-8i, and other
SAS2008-based HBAs.

## What this is NOT

Official Broadcom distribution. Broadcom no longer hosts SAS2008 firmware on
their CDN at consistent URLs. This mirror preserves the canonical bytes
extracted from authentic Broadcom / Dell / Lenovo / Intel installer packages
(verified against vendor catalog md5 claims where available).

## Schema (manifest.toml)

```toml
[firmware.<id>]
display     = "Human-readable description"
file        = "<canonical-filename>"     # path relative to repo root
size        = <bytes>
sha256      = "<64-hex>"                  # lsi-flash refuses to flash a mismatch
chip        = "SAS2008"
mode        = "IT" | "IR" | "IMR" | "BIOS" | "UEFI" | "UEFI-signed" | "UEFI-EBC" | "BIOS-LBF"
phase       = "20.00.07.00"               # MPT/MPT2BIOS firmware version
vendor      = "lsi" | "dell" | "intel" | "ibm" | ...
model       = "H200A" | "H310MB" | ...    # for OEM firmware
oem_rev     = "A09"                       # for Dell-packaged firmware
tier        = "primary" | "alternative" | "legacy"
source      = "where it came from"
```

```toml
[sbr.<id>]
display     = "..."
file        = "<canonical-filename.sbr>"
size        = 256
sha256      = "..."
chip        = "SAS2008"
vendor      = "..."
tier        = "..."
```

## Naming convention

Files use the pattern `<chip>-<vendor>[-<model>]-<version>[-<variant>].<personality>`:

- **Personality is the file extension** so files sort with related types grouped together
- `.it` IT-mode chip firmware
- `.ir` IR-mode chip firmware (Integrated RAID 0/1/1E/10)
- `.imr` Integrated MegaRAID composite (4.4 MB, H310 family)
- `.bios` Legacy PCI option-ROM BIOS
- `.uefi` UEFI BSD HII driver (option-ROM-wrapped)
- `.efi` UEFI BSD HII driver (raw shell-loadable)
- `.sbr` Subsystem boot record (256-byte EEPROM template)

Examples:

```
sas2008-lsi-p20-00-07-00.it          Stock LSI IT firmware Phase 20.00.07.00
sas2008-lsi-p19-00-00-00.ir          Stock LSI IR firmware Phase 19
sas2008-lsi-p20.bios                 Stock LSI legacy BIOS
sas2008-lsi-p20-signed.uefi          Stock LSI MS-signed UEFI driver (Secure Boot)
sas2008-dell-h200a-a09.ir            Dell PERC H200 Adapter rev A09 IR firmware
sas2008-dell-h310mb-a07.imr          Dell PERC H310 Mini Blade rev A07 IMR
sas2008-dell-ita-a04.it              Dell Internal Tape Adapter — Dell's blessed IT image
sas2008-dell-6gbps-a11.it            Dell 6Gbps SAS HBA (K161K) native IT-mode
sas2008-intel-rs2wc080-v20-12-1-0168.imr   Intel IMR firmware
sas2008-ibm-m1015.ir                 IBM ServeRAID M1015 restoration firmware
sas2008-fujitsu-d2607-a11.sbr        Fujitsu D2607 SBR template (rev A11)
sas2008-empty.sbr                    Blank SBR (Dell tamper bypass)
```

## Tiers

- **`primary`** — the recommended file for most users. The wizard defaults to these.
- **`alternative`** — older firmware revisions, sub-variants, EBC architecture, etc. Still validated, but the wizard hides them behind a "show more" option.
- **`legacy`** — kept for historical / debugging / edge-case purposes (e.g. `sas2008-ibm-m1015.ir` MegaRAID restoration firmware, pre-P7 LSI phases).

## Coverage

### Stock LSI chip firmware
- **IT** (Initiator-Target / HBA passthrough): 19 versions, P7 through P20.00.07.00
- **IR** (Integrated RAID 0/1/1E/10): 20 versions, P4 through P20.00.07.00
- **BIOS** option-ROM: 21 versions, MPT2BIOS-7.03 through MPT2BIOS-7.39.02
- **UEFI BSD HII driver**: 35 unsigned + signed variants across P15-P20
- **UEFI BSD HII (EBC arch-neutral)**: 15 variants
- **BIOS-LBF** (special payload format): 2 variants (P10, P13.5)

### Dell PERC OEM firmware (chip-level, extracted from Dell ZPE/DUP installers)
- **H200 Adapter**: A00, A03, A04, A09 (5 binaries)
- **H200 Integrated**: A03, A04, A05, A10 (4)
- **H200 Embedded** (PowerEdge blades): A02, A03, A07, A08 (4)
- **H200 Modular**: A00, A04, A08, A09 (4)
- **H310 Adapter** (full-size): A00, A02, A05 (×2 variants), A09, A11 (×3 variants) — 7 binaries, IMR personality
- **H310 Mini Blade**: A04, A05, A06, A07, A08, A10 (6 binaries, IMR)
- **H310 Mini Monolithic**: A11 (IMR)
- **ITA** (Internal Tape Adapter): A04 — Dell's blessed IT image for H200 hardware
- **6Gbps SAS HBA** (K161K): A04, A11 — Dell-native IT-mode HBA

### Other OEM firmware
- **Intel RS2WC080**: 1 IMR firmware (pkg v20.12.1-0168)
- **IBM ServeRAID M1015**: 1 MegaRAID-stack restoration firmware

### SBR templates (256-byte EEPROM identity)
- Stock LSI 9211-8i
- Dell H200 Adapter, H200 Embedded
- Dell H310 stock, H310 Mini-modded, H310 Full-modded (community)
- Fujitsu D2607 (A11 and A21 wiring variants)
- Empty (Dell tamper bypass)

## File integrity

Every binary in this repo has its sha256 in the manifest. The `lsi-flash` tool
refuses to flash a file whose hash doesn't match the manifest claim.

## Provenance

Each manifest entry's `source` field documents where the firmware came from
(Broadcom doc-ID, Wayback Machine snapshot, Dell catalog snapshot, IBM PSREF,
community mirror). The Round 4 collection methodology that produced this haul
is documented in lsi-flash-notes at
`references/methodology/round-4-firmware-collection.md`.

## Format references

Detailed binary format specifications for each personality are in the
lsi-flash-notes repo:

- `mpt-firmware-format.md` — `.it`, `.ir`, `.imr` chip firmware (MPI2_FW_HEADER)
- `mpt-bios-option-rom.md` — `.bios` legacy PCI option-ROM
- `uefi-bsd-hii-driver.md` — `.uefi`, `.efi` UEFI drivers (PE/COFF)
- `imr-megarec-format.md` — `.imr` 4MB composites (in progress)
- `sbr-format.md` — `.sbr` 256-byte EEPROM templates

These specs document the wire format, validator logic, and manipulation
recipes (SAS WWN patching, SubsysVID re-write, etc.).

## Reactive removal policy

If Broadcom, Dell, Fujitsu, IBM, HP, Intel, or any other rights-holder requests
removal of any file: contact the maintainer via GitHub issue; the file will be
removed from the next release. Historical commits will remain (cannot be
rewritten without breaking distributed clones) but the file will be deleted
from `main`.

## License

The `manifest.toml` schema, README, and accompanying tooling are MIT-licensed
(see `LICENSE.md`). The firmware binaries are proprietary to their respective
vendors (Broadcom, Dell, Intel, IBM, Fujitsu) and included here under
fair-use-for-preservation for the homelab cross-flashing community.
