# RT-BE92U Stock Firmware: SSH Security and Health Audit

Version 1.7.1 · 2026-10-18 · ASUS RT-BE92U · Asuswrt 3.0.0.6.102 (stock)

A printable runbook for auditing an ASUS RT-BE92U on stock Asuswrt over SSH,
from AyySSHush backdoor indicators and WAN exposure to firmware currency,
DNS-over-TLS, Samba guest access and USB SSD TRIM. Eighteen graded checks
each pair a check command with a pass rule and a fix; four INFO items record
a baseline. Target firmware: 3.0.0.6.102_40497 or newer, the newest ASUS
release as of 2026-10-18.

## Files

| File | What It Is |
| --- | --- |
| `rt-be92u-ssh-audit.pdf` | The runbook: 11 pages, US Letter |
| `README.md` | This file |
| `CHANGELOG.md` | Release history, newest first |

**PDF checksum.** 133,267 B, SHA-256
`fc66fafa5b88442d223dd1250d91745d33d4249ed7908758d03a201a13efe8de`.

## What Is Inside

Ten sections: scope; a summary table with severity, state and verdict rules;
the procedure; conventions; SSH setup and nvram snapshots; a read-only
collector; the checks; changes not to apply; limitations and open items; and
27 references.

| ID | Severity | Check |
| --- | --- | --- |
| C-01 | CRIT | SSH backdoor indicators (AyySSHush) |
| C-02 | CRIT | SSH reachable from WAN |
| C-03 | CRIT | Web UI reachable from WAN |
| H-01 | HIGH | Firmware currency |
| H-02 | HIGH | Telnet daemon |
| H-03 | HIGH | WPS |
| M-01 | MED | SSH password login |
| M-02 | MED | DoT certificate checks (Strict profile) |
| M-03 | MED | DNS rebind protection |
| M-04 | MED | Samba guest access |
| M-05 | MED | UPnP |
| L-01 | LOW | Browser auto-DoH bypass |
| L-02 | LOW | Web UI over plain HTTP |
| L-03 | LOW | Admin access allowlist |
| L-04 | LOW | Idle timeouts |
| L-05 | LOW | DDNS exposure |
| L-06 | LOW | SSH default port |
| L-07 | LOW | USB SSD TRIM |
| I-01 | INFO | Persistence, reboot schedule |
| I-02 | INFO | Boot-hook probe |
| I-03 | INFO | Thermals, radios, clients |
| I-04 | INFO | Listening services baseline |

## How to Use

Replace the placeholders listed in section 4.2, then follow section 3:

1. Set up the `be92u` SSH host alias and save a first nvram snapshot
   (section 5).
2. Run the read-only collector and keep its output (section 6).
3. Run C-01 before any other check. On any indicator, stop, follow its fix,
   then start again at step 1.
4. Run C-02 and C-03, then the HIGH, MED, LOW and INFO items; after each
   fix, re-run that item's check and any Verify step it lists.
5. Run the collector again, then take a second snapshot and diff the two
   (section 5).
6. After the next reboot, run the collector once more; every value must
   persist.
7. Fill in the STATE column and the verdict (section 2).

**Router** blocks run in BusyBox ash over `ssh be92u`, **host** blocks in
fish, and **GUI** steps at the menu path given. Snapshots hold secrets: keep
them on the host at mode 0600, never on the router's Samba share.

## Requirements

- **Router:** ASUS RT-BE92U on stock Asuswrt 3.0.0.6.102, with SSH reachable
  from the host; if SSH is off, turn it on as LAN only, otherwise change no
  SSH setting before C-01 and the first snapshot record it.
- **Host:** fish (blocks tested on 3.7), `ssh`, `ssh-keygen`, `sha256sum`,
  `diff`, `drill` (ldns) and, for M-04, `smbclient`.
- **USB SSD:** a current backup before L-07, whose check sends UNMAP.

## How to Verify

```fish
begin
    set -l want fc66fafa5b88442d223dd1250d91745d33d4249ed7908758d03a201a13efe8de
    set -l got (sha256sum rt-be92u-ssh-audit.pdf | string split -f1 -- ' ')
    test "$got" = "$want"; and echo MATCH; or echo MISMATCH
end
qpdf --check rt-be92u-ssh-audit.pdf
pdfinfo rt-be92u-ssh-audit.pdf | grep -E '^(Title|Pages|Page size):'
pdffonts rt-be92u-ssh-audit.pdf
```

Expect MATCH, no qpdf errors, 11 Letter pages and three embedded Liberation
fonts.

## Scope and Limits

Stock firmware only: Asuswrt-Merlin features and custom startup scripts are
out of scope, and I-02 only probes for a boot hook. Fixes use the web UI
wherever it offers the setting; raw nvram writes appear only where an item
gives them, and no item needs `nvram unset`, `nvram erase` or raw writes to
the flash partitions. GUI labels and nvram semantics come from ASUS's web-UI
code as carried in Asuswrt-Merlin's tree; stock firmware was not diffed, and
section 9 lists what is unverified on this model.

## Versioning

Semantic versioning; the release version is the one printed in the PDF.
`CHANGELOG.md` follows the kernel.org style, newest first.
