Changelog
=========

Newest first. Each version matches the one printed in the PDF.


1.7.0 - 2026-10-14
------------------

  - summary: LOW covers health too; ACCEPTED also covers a hardware or
    firmware limit
  - conventions: SSH_PORT is the router's current port, even 53282, which
    C-01 then flags
  - conventions: files on persistent storage (/jffs when writable, the
    SSD) survive a reboot, as committed nvram does
  - C-01: greps every /tmp/syslog.log* file in full; no dropbear line at
    all means logging is off, an indicator unless you turned it off
  - C-01: sshd_enable criterion left to C-02
  - C-01: WAN cut before evidence capture; a timestamped 0600 file keeps
    syslog, listeners, cron jobs, /jffs and processes
  - C-01: new key set as IdentityFile; stale host key cleared with
    ssh-keygen -R
  - H-02: raw fix applied with service restart_time; reboot only if port
    23 still listens, so section 4.3 lists no H-02 exception
  - L-01: passes on 0 while DoT is on, since Auto then blocks too; the
    summary row matches
  - L-07: the unmap fix reverts on re-plug as well as reboot, including
    the I-02 re-plug
  - I-04: falls back to netstat -lntu when netstat -lntup fails
  - do-not-apply: scoped to raw writes to the flash partitions
  - docs: README and CHANGELOG added


1.6.0 - 2026-10-14
------------------

  - scope: target moved to 102_40497 (2026-10-12); the H-01 pass rule
    and SHA-256 follow, so the recorded 102_40311 fails
  - procedure: a C-01 indicator restarts the run at step 1; persistence
    is re-checked after any reboot, 04:05 or manual
  - C-01: check counts stored keys; pass needs one fingerprint per key
  - C-01: fix saves the RAM-only syslog first, upgrades before the reset,
    rotates every secret the snapshots hold, reconnects the WAN last
  - C-01: the section 5 settings backup is never imported either
  - H-02: reboot at once rather than leave Telnet open until 04:05
  - M-02: Strict drops an upstream that fails authentication; with TLS
    as the only transport, an AdGuard outage stops DNS either way
  - M-04: each share listed anonymously with -U%; every listing must be
    refused with ACCESS_DENIED or LOGON_FAILURE
  - L-03: the host's type-3 entry must be enabled
  - L-06: passes on a port from 1024 to 65535 other than 53282
  - L-07: SSD backup now precedes the check, whose fstrim sends UNMAP;
    ACCEPTED also when TRIM fails after the fix
  - I-01: expected values named per key; an unset key prints nothing
  - limits: stock config and syslog paths listed as unverified


1.5.0 - 2026-10-08
------------------

  - conventions: if SSH is off, enable it LAN-only; otherwise change no
    SSH setting before C-01 and the first snapshot record it
  - setup: host alias pins IdentityFile and IdentitiesOnly; the GUI
    backup is saved with the remove-password box unticked
  - checks: C-01 and H-02 listener checks use netstat -lnt
  - C-01: ssh-keygen's message for an empty key list explained
  - C-03: models with HTTPS, this one included, allow only HTTPS from WAN
  - H-01: version compared field by field, numerically, suffix ignored
  - H-03: both enable keys must read 0, not empty; empty WPS mode keys
    are ignored by the pass rule and the fix
  - M-02: check reads the transport list and skips comments; Strict
    needs TLS as the only transport
  - M-02: raw fix switches the profile only; verify queries an uncached
    random name, and SERVFAIL triggers the rollback
  - M-04: router check lists the share names; a protocol-negotiation
    error proves nothing
  - L-01: Auto blocks only while DNS Privacy or global DNS Filter is on
  - L-03: allowlist holds at most four entries
  - I-03: the RT-AX58U v2 testing claim now refers to the script's
    collection


1.4.0 - 2026-10-08
------------------

  - summary: the severity table became a legend under the summary table;
    the command-location and persistence tables became text
  - procedure: INFO items join the run order; every fix re-runs its check
  - checks: Verify lines kept only where they add a test
  - H-01: duplicate SHA-256 text block removed
  - do-not-apply, limits: entries that repeated C-01 and M-01 removed
  - refs: 32 cut to 27 with the text that cited them (Censys, BusyBox,
    Netstat tool, Protect Service, hardware page); renumbered
  - doc: revision section removed; printed date corrected from
    2026-10-09 to 2026-10-08


1.3.0 - 2026-10-08
------------------

  - procedure: moved to section 3
  - conventions: menu paths and labels follow ASUS's English UI strings
  - setup: commands use the be92u alias directly; snapshots stored under
    XDG_DATA_HOME
  - C-03: both WAN web ports checked
  - M-02: an SPKI pin now satisfies the hostname check; the Strict fix
    restarts stubby only
  - M-04: graded by anonymous access; SMBv1 no longer graded
  - L-07: TRIM that works only after the fix is ACCEPTED
  - refs: regrouped


1.2.0 - 2026-10-08
------------------

  - collector: covers every single-value key the checks read
  - checks: M-05 and L-05 exceptions recorded as ACCEPTED
  - C-01: hard-reset procedure sourced
  - M-01: key login confirmed before password login is turned off
  - M-02: AdGuard server list cited
  - L-03: fix also adds every other device that manages the router
  - L-07: passes on a clean fstrim exit, 0 bytes trimmed included; a
    missing fstrim applet is ACCEPTED


1.1.0 - 2026-10-08
------------------

  - summary: result states and verdict rules added
  - conventions: prerequisites added
  - setup: be92u SSH host alias added; failed snapshots kept as .part
    files
  - C-01: the fix's settings-only reset became a hard factory reset; the
    image is fetched on another machine and its SHA-256 checked
  - H-01: newer builds pass; the fix adds the 102_38961 password policy
  - M-02: DoT hostname check automated
  - I-01: reboot-scheduler type recorded


1.0.0 - 2026-10-08
------------------

  - doc: first issue, 22 checks (3 CRIT, 3 HIGH, 5 MED, 7 LOW, 4 INFO)
    for stock Asuswrt 3.0.0.6.102_40311
