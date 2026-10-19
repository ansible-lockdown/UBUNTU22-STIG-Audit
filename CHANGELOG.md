# Changelog

## 2.9.0 - Based on STIG V2R9 - 2026 October Updates - check fixes, V2R9 metadata, syver, goss documentation link

- **`run_audit.sh` now runs [syver](https://github.com/krameff/syver), replacing goss.** Syver is the
  signed continuation of the krameff goss fork and reads this content unchanged. `AUDIT_BIN` defaults
  to `/usr/local/bin/syver` and `AUDIT_BIN_MIN_VER` moves from `0.4.4` to `0.13.0`, so a goss binary
  now fails the version pre-check, and the pre-check messages name the audit binary rather than goss.
  The paired remediation role installs syver v0.13.0 and passes it as `AUDIT_BIN`; it must carry that
  change before this one is used. The README requirements and links describe syver
- **Nine checks failed compliant hosts.** UBTU-22-211000 (Ubuntu 22.04 LTS must be a
  vendor-supported release.) wrote its exit status as a list, which syver rejects as a matcher error,
  and required both `attached: true` and `attached: false`; it now runs `pro status` as the check text
  does and expects an exit status of 0 and an `attached:` line, without requiring attachment.
  UBTU-22-271025 (Ubuntu 22.04 LTS must initiate a graphical session lock after 10 minutes of
  inactivity.) lacked the closing `/` on its `idle-delay` pattern, so it was matched as literal text
  and never passed; it now labels each `gsettings` value and accepts an `idle-delay` of 1 to 600.
  UBTU-22-412030 (Ubuntu 22.04 LTS must automatically exit interactive command shell user sessions
  after 15 minutes of inactivity.) failed a `TMOUT` below 900 and passed `TMOUT=9000`; it now accepts
  1 to 900 and rejects 0 or anything above 900. UBTU-22-412035 (Ubuntu 22.04 LTS default filesystem
  permissions must be defined in such a way that all authenticated users can read and modify only
  their own files.) required a single space in `UMASK 077`, failing the stock tab-separated
  `login.defs`. UBTU-22-612025 (Ubuntu 22.04 LTS must electronically verify personal identity
  verification (PIV) credentials.), UBTU-22-612030 (Ubuntu 22.04 LTS, for PKI-based authentication,
  must validate certificates by constructing a certification path (which includes status information)
  to an accepted trust anchor.) and UBTU-22-612035 (Ubuntu 22.04 LTS for PKI-based authentication,
  must implement a local cache of revocation data in case of the inability to access revocation
  information via the network.) anchored `cert_policy` at the start of the line, which the indented
  `pkcs11_module` block never is, and the first two also required their token to be the last one on
  the line; UBTU-22-612035 also carried a `(?!` lookahead, which RE2 rejects, so it errored on every
  host. Each now matches its token anywhere in the `cert_policy` list. UBTU-22-631010 (Ubuntu 22.04
  LTS must use DOD PKI-established certificate authorities for verification of the establishment of
  protected sessions.) expected the literal text `^DOD_PKE_CA_chain.pem`; it now passes when a file
  name contains `dod`, case-insensitively. UBTU-22-631015 (Ubuntu 22.04 LTS must be configured such
  that Pluggable Authentication Module (PAM) prohibits the use of cached authentications after one
  day.) anchored on a line start that `grep` prefixes with the file name, and an empty `conf.d` made
  `grep` exit 2; it now uses `grep -h` and accepts exit 2
- **Four negated patterns rejected nothing.** Those in UBTU-22-412035, UBTU-22-411035 (Ubuntu 22.04
  LTS must disable account identifiers (individuals, groups, roles, and devices) after 35 days of
  inactivity.), UBTU-22-612025 and UBTU-22-612030 were malformed (`{2;}` with no closing `/`, no
  slashes at all, and a trailing `;` in the last two), so they were literal text. UBTU-22-412035 now rejects any
  `UMASK` whose group or other digit is not `7`, UBTU-22-411035 rejects `INACTIVE=-1`, and
  UBTU-22-612025 fails when any `cert_policy` line in the `opensc` block lacks `ocsp_on`, as the check
  text requires of every line
- **Ten checks tested the wrong thing or less than the check text covers.** UBTU-22-213015 (Ubuntu
  22.04 LTS must disable kernel core dumps so that it can fail to a secure state if system
  initialization fails, shutdown fails or aborts fail.) tested `kdump.service`, which does not exist;
  it now tests `kdump-tools.service`, which the check text names, and passes a host without
  `kdump-tools` installed, which the check text says is not a finding. UBTU-22-612020 (Ubuntu 22.04 LTS
  must implement smart card logins for multifactor authentication for local and network access to
  privileged and nonprivileged accounts.) looked for `pam_pkcs11.so` in `common-password`, with
  `[success=2 default=ignore]` read as a one-character class; it now reads `common-auth`, as the
  check does. UBTU-22-232026 (Ubuntu 22.04 LTS must generate error messages that provide information
  necessary for corrective actions without revealing information that could be exploited by
  adversaries.) excluded `*syslog`, which the check does not, and flagged `eipp.log.xz`, which the
  V2R9 note says is not a finding. UBTU-22-232027 (Ubuntu 22.04 LTS must generate system journal
  entries without revealing information that could be exploited by adversaries.) flagged every
  `2750` journal directory and never checked the journal files; it now flags directories more
  permissive than `2750` and files more permissive than `0640`. Its `tmpfiles.d` patterns still expect
  the `2640` the paired remediation writes, not the V2R9 `2750`. UBTU-22-232075 (Ubuntu 22.04 LTS
  library files must be group-owned by "root".) searched `*.so` where the check searches `*.so*`, so
  versioned libraries were never checked. UBTU-22-653065 (Ubuntu 22.04 LTS must be configured so that
  audit configuration files are not write-accessible by unauthorized users.) only flagged files with
  group read, so a world-writable `0606` file passed. UBTU-22-651030 (Ubuntu 22.04 LTS must use
  cryptographic mechanisms to protect the integrity of audit tools.) passed commented-out `aide.conf`
  lines. UBTU-22-255050 (Ubuntu 22.04 LTS must configure the SSH daemon to use FIPS 140-3-approved
  ciphers to prevent the unauthorized disclosure of information and/or detect changes to information
  during transmission.) still listed `aes192-ctr`, which the V2R9 list does not include, in the
  standalone `vars/STIG.yml` value. It, UBTU-22-255055 (Ubuntu 22.04 LTS must configure the SSH daemon to use
  Message Authentication Codes (MACs) employing FIPS 140-3-approved cryptographic hashes to prevent
  the unauthorized disclosure of information and/or detect changes to information during
  transmission.) and UBTU-22-255060 (Ubuntu 22.04 LTS SSH server must be configured to use only
  FIPS-validated key exchange algorithms.) now anchor the end of the line, so an extra algorithm fails
- **UBTU-22-651015 (Ubuntu 22.04 LTS must configure Advanced Intrusion Detection Environment (AIDE) to
  perform file integrity checking on the file system.) produced no result by default.** Its
  `command:` key sat inside the `ubtu22stig_allow_aide_check` branch, so with the default `false` the
  manual test became an unknown top-level key and was dropped. With the check disabled it now reports
  a manual result that fails, as the other manual checks in this content do
- **Metadata now matches the V2R9 XCCDF.** 38 `CCI` entries across 34 controls were wrong, malformed
  (`CCI-00795`) or missing an XCCDF CCI, and 11 `Group_Title` entries across 10 controls did not match
  the group SRG plus the rule's `Satisfies:` list. 11 blocks carried the `STIG_ID` of another control, among them the
  `pam_pkcs11.so` block of UBTU-22-612020, which reported under UBTU-22-611045 (Ubuntu 22.04 LTS must
  be configured so that when passwords are changed or new passwords are established, pwquality must
  be used.). UBTU-22-213015 carried another rule's `Vul_ID`, UBTU-22-653035 (Ubuntu 22.04 LTS must
  allocate audit record storage capacity to store at least one weeks' worth of audit records, when
  audit records are not immediately sent to a central audit record storage facility.) carried `Cat:
  2` for a low-severity rule, and the UBTU-22-611055 (Ubuntu 22.04 LTS must store only encrypted
  representations of passwords.) title began with UBTU-22-611045

- **the goss manual link still pointed at the pre-fork repository.** `README.md` referenced
  `aelsabbahy/goss/blob/master/docs/manual.md#patterns`, left behind when the rest of this file
  moved to the krameff fork. It now points at `github.com/krameff/goss/blob/devel/docs/index.md`.
  The `#patterns` anchor is dropped rather than carried over: the fork's documentation is laid out
  differently, and an anchor that cannot be confirmed is worse than a working link to the page

## Code quality: krameff goss compatibility

- run_audit.sh: parse only the first line of `goss -v` (the krameff fork ships a 2-line version banner) so the version gate reads the version correctly instead of the copyright line.
- README.md: point the goss reference at the krameff fork.

## benchmark_v2.9.0 (V2R9 alignment - STIG V2R9, 01 July 2026)

V2R8 -> V2R9 is updates-only (188 rules unchanged; 0 added, 0 removed; 1 severity change; 11 SV-* revision drifts).

V2R9 benchmark alignment:
- vars/STIG.yml: benchmark_version v2.8.0 -> v2.9.0
- run_audit.sh: BENCHMARK_VER 2.8.0 -> 2.9.0
- README.md: v2.8.0 reference + V2R8 download URL -> v2.9.0

Severity change:
- UBTU-22-215040: HIGH (CAT1) -> MEDIUM (CAT2). Moved goss check cat_1/UBTU-22-215040.yml -> cat_2/UBTU-22-21xxxx/; removed the nfs-common check block (V2R9 finding covers nfs-kernel-server only); toggle moved to the CAT2 section in vars/STIG.yml.

Content updates:
- UBTU-22-271025: idle-delay check `uint32 900` -> `uint32 600` (V2R9 updates the GNOME idle delay to 10 minutes); title 15 -> 10 minutes.
- UBTU-22-432010: check already aligned (tests `!authenticate` only; V2R9 removed NOPASSWD from the Check text).

Rule_ID metadata sync (11 SV-* revision drifts in V2R9):
- cat_2/UBTU-22-21xxxx/UBTU-22-215040.yml: SV-279937r1156364_rule -> SV-279937r1208676_rule
- cat_1/UBTU-22-255025.yml: SV-260526r991591_rule -> SV-260526r1208678_rule
- cat_2/UBTU-22-25xxxx/UBTU-22-255030.yml: SV-260527r986275_rule -> SV-260527r1208681_rule
- cat_2/UBTU-22-25xxxx/UBTU-22-255035.yml: SV-260528r970703_rule -> SV-260528r1208803_rule
- cat_1/UBTU-22-255040.yml: SV-260529r991589_rule -> SV-260529r1208685_rule
- cat_2/UBTU-22-25xxxx/UBTU-22-255045.yml: SV-260530r991589_rule -> SV-260530r1208687_rule
- cat_2/UBTU-22-25xxxx/UBTU-22-255050.yml: SV-260531r1155212_rule -> SV-260531r1208689_rule
- cat_2/UBTU-22-25xxxx/UBTU-22-255060.yml: SV-260533r958408_rule -> SV-260533r1208690_rule
- cat_2/UBTU-22-25xxxx/UBTU-22-255065.yml: SV-260534r958510_rule -> SV-260534r1208804_rule
- cat_2/UBTU-22-27xxxx/UBTU-22-271025.yml: SV-260538r1069119_rule -> SV-260538r1208695_rule
- cat_2/UBTU-22-432xxx/UBTU-22-432010.yml: SV-260558r1155216_rule -> SV-260558r1208696_rule

## benchmark_v2.8.0 (V2R8 alignment - STIG V2R8, 01 April 2026)

V2R7 -> V2R8 is updates-only (188 rules unchanged; 0 added, 0 removed, 0 severity changes; 7 SV-* revision drifts).

V2R8 benchmark alignment:
- vars/STIG.yml: benchmark_version v2.7.0 -> v2.8.0
- run_audit.sh: BENCHMARK_VER 2.7.0 -> 2.8.0
- README.md: v2.7.0 reference + V2R7 download URL -> v2.8.0

Cleanups:
- vars/STIG.yml: removed duplicate `ubtu22stig_215040` toggle (rule is HIGH/CAT1 per V2R8 XCCDF; was redundantly listed under both CAT1 and CAT2 in the audit vars file)

Rule_ID metadata sync (7 SV-* revision-suffix drifts in V2R8):
- cat_2/UBTU-22-23xxxx/UBTU-22-232080.yml: Rule_ID SV-260501r958566_rule -> SV-260501r1184052_rule
- cat_2/UBTU-22-23xxxx/UBTU-22-232085.yml: Rule_ID SV-260502r958566_rule -> SV-260502r1184054_rule + title typo UBTU-22-232080 -> UBTU-22-232085 fixed
- cat_2/UBTU-22-23xxxx/UBTU-22-232090.yml: Rule_ID SV-260503r958566_rule -> SV-260503r1184056_rule
- cat_2/UBTU-22-23xxxx/UBTU-22-232095.yml: Rule_ID SV-260504r958566_rule -> SV-260504r1184058_rule
- cat_2/UBTU-22-25xxxx/UBTU-22-251020.yml: Rule_ID SV-260516r991593_rule -> SV-260516r1184061_rule
- cat_2/UBTU-22-25xxxx/UBTU-22-255020.yml: Rule_ID SV-260525r958390_rule -> SV-260525r1184064_rule (both check_login_banner and sshd_banner blocks)
- cat_1/UBTU-22-271030.yml: Rule_ID SV-260539r1069103_rule -> SV-260539r1184066_rule

## benchmark_v2.7.0 QA (May 2026)

QA cycle fixes (no rule additions or removals):

- vars/STIG.yml: benchmark_version corrected from 2.6.0 to v2.7.0 (aligns with audit branch name and remediation defaults)
- vars/STIG.yml: bootloader_password_hash comment marker cleaned up (## -> #)
- README.md: removed RHEL9 template leakage ("STIG RHEL9 based servers" -> "STIG Ubuntu 22.04 based servers")
- CONTRIBUTING.md: header "MindPoint Group Projects" -> "Ansible-Lockdown Projects" (and DCO body wording)
- LICENSE: Mindpoint -> MindPoint casing; copyright year 2025 -> 2026
- run_audit.sh: added Ubuntu detection branch and BENCHMARK_OS fallback for empty OS detection; dropped redundant `grep -w` from VERSION_ID grep (broke on non-GNU greps)
- Renamed Changelog.md -> CHANGELOG.md (case standardisation)

## 20th Feb 2026 V2R7 (align to STIG V2R7)

- UBTU-22-254025: Rule removed
- Lint
- audit alignment
- company name
- RuleID updates + comments
  - 211000 - extended checks
  - 212010
  - 212015
  - 213010
  - 213015
  - 232035
  - 232050
  - 232110
  - 232145
  - 254010 - Added NFS not installed
  - 254030
  - 255050
  - 291010
  - 291015
  - 432010
  - 432011
  - 651015
  - 654041
  - 654055
  - 654060
  - 631015
  - 653025

## 11th Feb 2026 - based on v2r6

- pre-commit-update
- workflow updates
- pre status update thanks to @kurtcorsha
- sudo group variable moved to correct section in defaults/main.ym


## 2nd December 2025 - based on v2r6

Rule updates
- UBTU-22-211000 - New control
- UBTU-22-212015
- UBTU-22-232026
- UBTU-22-651015

several other improvements
- ssh kex,macs and cipher logic
- 232026, 232080, 232090, 232140 fixed stig consistency errors
- 214010 - logic update
- 411045 - rewritten due to missing steps in STIG documentation for common-account

### 23 October 2025 - based on STIG v2r5
Control updates from Version 2 Release 3 through Version 2 Release 5
- New README layout
- New Workflows

Rules updates

CAT1
- UBTU-22-271030
- UBTU-22-432015 - updated logic
- UBTU-22-611060
  - Added loop

CAT2
- UBTU-22-212015
- UBTU-22-213015
- UBTU-22-232020
  - extended find locations
- UBTU-22-232070
  - extended find locations
- UBTU-22-232075
  - extended find locations
- UBTU-22-232110
- UBTU-22-253010
- UBTU-22-254010 - New control
- UBTU-22-254015 - New control
- UBTU-22-254020 - New control
- UBTU-22-254030 - New control
- UBTU-22-271020
- UBTU-22-271025
- UBTU-22-432010
- UBTU-22-432011 - New control
- UBTU-22-611055
- UBTU-22-612020
- UBTU-22-612030
- UBTU-22-651015
- UBTU-22-651030
- UBTU-22-653030
- UBTU-22-653065
- UBTU-22-653075
  - Added loop control
- UBTU-22-654041 - New control
- UBTU-22-654224 - New control

CAT3
- UBTU-22-252010
- UBTU-22-252015
- UBTU-22-254025 - New control
- UBTU-22-411045 rewritten
- UBTU-22-412015 - Removed
- UBTU-22-653020
- UBTU-22-653035 conditional fix
## Based on STIG v2r2
### 1.0.0

Updated lint configs
spacing aligned
lint updates

### Initial
