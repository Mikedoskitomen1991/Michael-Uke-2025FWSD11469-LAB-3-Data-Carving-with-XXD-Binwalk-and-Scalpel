Data Carving with XXD, Binwalk and Scalpel Report

Student Name: Michael Uke
Environment: Kali Linux
Primary Evidence: Ch01InChap01.dd
Additional Evidence: J_ub_law.jpg; 120M.7z / usb_fat_carving.001
Tools: The Sleuth Kit, xxd, strings, file, antiword, MD5/SHA-256, Binwalk and Scalpel
Date: September 2026
1. Introduction
This laboratory involved forensic examination of digital evidence using Kali Linux. The work covered evidence preservation and hashing, JPEG hexadecimal analysis, strings and metadata examination, filesystem identification, deleted-file discovery and recovery, sector level analysis, USB evidence identification, and preparation for JPEG file carving. Original evidence was kept separate from working and recovery outputs.
2. Forensic Workspace
A dedicated workspace was created at ~/Forensics_Project_Lab3 with separate evidence, hashes, working, recovered, outputs, screenshots, logs and report areas. This separation helped preserve original evidence and distinguish recovered material from source evidence.
3. JPEG Evidence J_ub_law.jpg
The JPEG evidence was examined without altering the original. The recorded file size was 581,331 bytes. MD5: 2e38edb92662bd14a6eca813bfc5572f. SHA-256: 0d16ea7c5523c68b57abb568cbb91fd116f8bdb5830b863bb2ea2acafcfed7e4. The image was identified as JPEG/JFIF, 1920 × 1080.
xxd confirmed the JPEG Start of Image marker FF D8 and JFIF information.
The file ended with FF D9, the JPEG End of Image marker.
A plain hexadecimal dump was created and reversed to reconstruct J_ub_law_reconstructed.jpg.
strings revealed JFIF and ICC colour-profile strings including ICC_PROFILE, acsp, rXYZ, gXYZ, bXYZ and related profile data.
No readable camera make/model, software, GPS/location, person/name or clear date/time information was identified in the targeted strings.
4. Main Forensic Image Ch01InChap01.dd
img_stat identified Ch01InChap01.dd as a raw forensic image. The image size was 1,474,560 bytes and the sector size was 512 bytes.
Property	Result
Image type	Raw
Image size	1,474,560 bytes
Sector size	512 bytes
Filesystem	FAT12
Filesystem start	Sector 0
Filesystem range	Sectors 0–2879
mmls did not return partition-table information. fsstat subsequently established that the image itself contained a FAT12 filesystem beginning at sector 0, so no additional partition offset was required.
5. Deleted Files and Recovery
Recursive fls analysis identified four deleted files. Each was recovered with icat and stored in the separate recovered directory.
File	Entry/Inode	Status	Sectors / Size
Billing Letter.doc	8	Deleted	237–283 / 24,064 bytes
confirmation.txt	11	Deleted	284 / 227 bytes
letter1.txt	15	Deleted	312 / 121 bytes
Regrets.doc	17	Deleted	313–358 / 23,552 bytes
file verified the recovered types: the two .doc files were Microsoft Composite Document File V2 files, while the two .txt files were ASCII text.
6. Recovered Evidence Findings
confirmation.txt contained FTP related correspondence including a server hostname, username, password and upload instructions. Sensitive credentials were not reproduced in this report.
letter1.txt contained a work-related message addressed to Earl requesting a meeting on 18 August.
Billing Letter.doc contained correspondence dated 13 October 2005 concerning domain registration, website hosting, a $500 registration fee and payment conditions.
Regrets.doc contained correspondence dated 2 November 2005 concerning the purchase of five domain-name variations.
7. Metadata and Sector Level Analysis
istat confirmed that all four deleted entries were not allocated and provided their actual short names, sizes and sector locations. Because the FAT12 filesystem begins at sector 0, blkcat analysis used -o 0 and the actual sectors reported by istat.
Billing Letter.doc: entry 8; sectors 237–283.
confirmation.txt: entry 11; sector 284.
letter1.txt: entry 15; sector 312.
Regrets.doc: entry 17; sectors 313–358.
8. USB Evidence usb_fat_carving.001
During examination of the extracted 120M evidence, the actual USB carving evidence file was identified as usb_fat_carving.001 under working/120M_extracted/120M/. It was approximately 119 MB. An initial attempt to hash a placeholder name, USB_IMAGE, failed; directory inspection identified the correct filename.
MD5: ba4a1d0ba49f4a6667b00a3b3e85e604
SHA-256: 9bfe4b5634ade30764f0e581a4686930f7fab0472378595b789b0cdb248c91d9
9. Scalpel File Carving Analysis
Scalpel 1.60 was available at /usr/bin/scalpel. The initial carving attempt failed because no file types were enabled. JPEG signatures for EXIF and JFIF were then enabled in scalpel.conf. A later attempt failed because the output directory was non empty, so a clean scalpel_output_2 directory was created. A final recovered file count was not recorded in the available evidence and is therefore not claimed.
10. Challenges Encountered
mmls did not initially display a partition table; fsstat established a FAT12 filesystem beginning at sector 0.
A placeholder USB_IMAGE filename caused an initial hashing error; the correct usb_fat_carving.001 file was then identified.
Scalpel initially reported that no file types were configured; JPEG signatures were enabled.
Scalpel rejected a non empty output directory; a clean output directory was created.
A sudo authentication issue occurred during the Scalpel stage.
11. Forensic Integrity and Limitations
The investigation maintained separation between original evidence, working files and recovered material; cryptographic hashes were recorded; and actual filesystem, inode and sector values were obtained from forensic tool output rather than guessed. Sensitive credentials discovered in recovered evidence were not unnecessarily reproduced. The available record does not contain a final Scalpel recovery count, so no unsupported carving result is included.
12. Conclusion
The laboratory successfully demonstrated evidence preservation, hashing, JPEG structure analysis, strings examination, FAT12 filesystem identification, deleted file discovery and recovery, metadata and sector analysis, USB evidence identification and file carving preparation. Four deleted files were identified and recovered from Ch01InChap01.dd. The exercise also demonstrated the importance of verifying evidence filenames, using clean output directories, recording hashes, protecting sensitive information and relying on actual forensic tool output.
