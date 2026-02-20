# Changelog

## V2R7 (align to STIG V2R7)

- **Removed** UBTU-22-254025: Rule removed in XML (duplicate requirement per v2r7_history). Deleted audit file `cat_3/UBTU-22-25xxxx/UBTU-22-254025.yml`, removed `ubtu22stig_254025` from vars/STIG.yml.
- **Align (Rule_ID / Vul_ID only):** Updated audit YML meta to match V2R7 XML Rule_ID and Vul_ID for: UBTU-22-211000, 212010, 212015, 213010, 213015, 232035, 232050, 232110, 232145, 254010, 254030, 255050, 291010, 291015, 432010, 432011, 651015, 654041, 654055, 654060, 631015, 653025.
- **Align (titles):** Synced all audit YML title lines to exact V2R7 XML Rule titles via `scripts/sync_titles_from_xml.py` (185 files). Check script passes: Rule_ID, Vul_ID, and title match XML.
