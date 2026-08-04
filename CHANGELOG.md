# Changelog

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
