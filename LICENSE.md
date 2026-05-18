# License Notice

## Software Components (MIT)

The `lsi-flash` tool source code, manifest reader, and any scripts in this repository are released under the MIT License:

```
Copyright (c) 2026 Matthew Jackson

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## Firmware Binaries (Proprietary - Fair Use for Preservation)

All firmware binaries included in this repository (`*.bin`, `*.fw`, `*.rom` files) are the proprietary intellectual property of their respective vendors:

- **Broadcom/LSI** — SAS2008 ROC firmware (2118it.bin, 2118ir.bin, mptsas2.rom, etc.)
- **Dell** — PERC H200/H310 OEM firmware (DELL_6GBPSAS.FW, DELL_mptsas2.rom)
- **Fujitsu/IBM** — SBR templates (SBR-A11.bin, SBR-A21.bin, sbrempty.bin)

These binaries are included in this repository under a **fair-use-for-preservation** rationale:

1. Broadcom no longer distributes SAS2008 firmware publicly; official download portals require support contracts or are archived
2. The community cross-flashing hobby requires these files to keep legacy hardware functional
3. This repository does not claim ownership or redistribution rights over the binaries
4. Files are provided "as-is" with SHA256 checksums for integrity verification only
5. A reactive removal policy is in place: any rights-holder may request file removal via GitHub issue

This is **not** an assertion of legal right to redistribute proprietary firmware. It is a community preservation effort acknowledging the legal grey area while prioritizing hardware longevity for homelab/research use cases.

## SBR Templates

SBR (Subsystem Boot Record) templates are community-derived configuration files based on reverse engineering of OEM firmware images. They contain PCI Subsystem Vendor ID / Product ID values and hardware configuration bytes extracted from actual cards. These do not contain copyrighted code but rather hardware identification data necessary for cross-flashing procedures.

## Attribution

Firmware sources are cited in `manifest.toml` under each entry's `source` field, typically referencing:
- lrq3000/lsi_sas_hba_crossflash_guide GitHub archive (Fair Use for preservation claim)
- FOHDeesha PERC cross-flashing guide
- Archive.org mirrors of legacy firmware packages

## Disclaimer

This repository is not affiliated with Broadcom, Dell, Fujitsu, IBM, or any other hardware vendor. Users assume all risk when flashing firmware to their hardware. Improper flashing can brick devices. Always verify checksums before use and follow established cross-flashing procedures from trusted community sources.
