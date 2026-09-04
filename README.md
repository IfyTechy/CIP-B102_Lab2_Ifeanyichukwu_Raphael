# Practical Lab 4 — USB Image Acquisition and Hash Verification

**Course:** SBT-DF202 — Computer and Digital Forensics  
**Institution:** International Cybersecurity & Digital Forensics Academy (ICDFA)  
**Lab Identifier:** CIP-B102-Lab4  
**Examiner:** Nebeuwa Ifeanyichukwu Raphael (ID: 202520850LE)  
**Instructor:** Aminu Idris  
**Acquisition Platform:** Kali Linux (x86_64)  
**Date of Acquisition:** August 31, 2026  

---

## Executive Summary

This repository contains the forensic evidence, acquisition logs, cryptographic hash verifications, and complete documentation for **Practical Lab 4: USB Image Acquisition and Hash Verification** under standard digital forensic protocols ($ISO/IEC$ 27037).

The primary objective was to acquire an authentic, bit-for-bit raw disk image (`.dd`) of a physical removable USB flash drive (`/dev/sdb`), establish immutable chain-of-custody tracking via MD5 and SHA-256 cryptographic hashing, and validate the evidence structure using the Autopsy Forensic Browser v2.24.

---

## Practical Objectives & Demonstrations

- **Forensic Disk Imaging Rationale:** Demonstrating the necessity of bit-stream physical copies over file-system logical copies.
- **Evidence Preparation & Handling:** Preparing controlled removable media under `/mnt/usb/CIP-B102-Lab4-Evidence/` with defined student identity file (`ifeanyichukwu_raphael.txt`).
- **Block Device Identification:** Isolating target block storage nodes (`lsblk`, `fdisk -l`, `df -h`) to prevent host data destruction.
- **Bit-Stream Image Acquisition:** Executing raw imaging using GNU coreutils `dd` on Kali Linux with block sizing (`bs=4M`) and error handling (`conv=noerror,sync`).
- **Cryptographic Integrity Verification:** Generating and comparing MD5 and SHA-256 hash digests across source hardware (`/dev/sdb`) and output `.dd` files.
- **Forensic Image Content Validation:** Mounting and parsing the acquired raw image within Autopsy v2.24 to extract evidence and verify FAT32 volume structures.

---

## Repository Contents

* [CIP-B102_Lab4_USB Image Acquisition and Hash Verification.pdf](./CIP-B102_Lab4_USB%20Image%20Acquisition%20and%20Hash%20Verification.pdf) – Official Lab Report
* `README.md` – Project Overview

---

## Target Device Specifications

| Parameter | Technical Specification / Forensic Record |
| :--- | :--- |
| **Case / Lab ID** | CIP-B102-Lab4 |
| **Examiner Name** | Ifeanyichukwu Raphael |
| **Target Device** | Removable Flash Disk |
| **Logical Block Node** | `/dev/sdb` (Partition: `/dev/sdb1`) |
| **Total Capacity** | 14.65 GiB / 15,728,640,000 Bytes |
| **File System** | FAT32 Partition Table |
| **Output Image File** | `CIPB102_Lab4_Ifeanyi_Raphael_USB.dd` |

---

## Execution Methodology & Commands

### Phase 1: Isolated Workspace Construction
```bash
mkdir -p ~/ICDFA_Forensics/Ch01InChap01/{original,working,recovered,hashes,screenshots,reports}
```

### Phase 2: Evidence Directory & Identity File Setup
sudo mkdir -p /mnt/usb
sudo mount /dev/sdb1 /mnt/usb
sudo mkdir -p /mnt/usb/CIP-B102-Lab4-Evidence
cd /mnt/usb/CIP-B102-Lab4-Evidence
sudo nano ifeanyichukwu_raphael.txt
cd ~
sudo umount /mnt/usb

### Phase 3: Bit-Stream Acquisition (dd)
```bash
sudo dd if=/dev/sdb of=~/ICDFA_Forensics/Ch01InChap01/original/CIPB102_Lab4_Ifeanyi_Raphael_USB.dd bs=4M status=progress conv=noerror,sync
```

### Cryptographic Integrity Verification
To guarantee zero data modification, MD5 and SHA-256 hashes were calculated for both the physical device (/dev/sdb) and the acquired image (.dd).
```bash
# Hash Generation Commands
sudo md5sum /dev/sdb | tee ~/ICDFA_Forensics/Ch01InChap01/hashes/source_md5.txt
sudo sha256sum /dev/sdb | tee ~/ICDFA_Forensics/Ch01InChap01/hashes/source_sha256.txt

md5sum CIPB102_Lab4_Ifeanyi_Raphael_USB.dd | tee ~/ICDFA_Forensics/Ch01InChap01/hashes/image_md5.txt
sha256sum CIPB102_Lab4_Ifeanyi_Raphael_USB.dd | tee ~/ICDFA_Forensics/Ch01InChap01/hashes/image_sha256.txt
```

