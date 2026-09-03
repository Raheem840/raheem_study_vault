---
chapter: 20
title: Security and Administration
source: "Connolly & Begg, Database Systems, 6th Ed, Ch.20.1–20.2, 20.6"
course: "[[00 - Course Map|CSC 2107 - Databases]]"
tags: [databases, csc2107, exam-material, security]
---

# Ch.20 — Security and Administration

## 1. Why this matters
Ties directly back to [[Chapter 07 - SQL Data Definition|Ch.7's]] GRANT/REVOKE syntax and gives it conceptual grounding — plus a testable list of threats and countermeasures.

---

## 2. Database security — definition and threats
**Database security**: the mechanisms that protect the database against intentional or accidental threats, covering confidentiality, integrity, and availability of data.

### Categories of threat (guaranteed list)
- **Theft and fraud** — unauthorized access for malicious data extraction or manipulation.
- **Loss of confidentiality** — sensitive data exposed to unauthorized parties.
- **Loss of privacy** — personal data about individuals exposed.
- **Loss of integrity** — data corrupted or made invalid (accidentally or maliciously).
- **Loss of availability** — data or the system becomes inaccessible to legitimate users (e.g. denial-of-service).

> **Exam trap**: threats can be **accidental** (hardware/software failure, human error) OR **deliberate** (malicious attacks) — a good exam answer distinguishes these, since the countermeasures differ (e.g. backups address accidental loss; access control addresses deliberate theft).

---

## 3. Countermeasures — computer-based controls (guaranteed list, know each precisely)
| Control | What it does |
|---|---|
| **Authorization** | The granting of a right or privilege to a user, allowing them legitimate access to the system/data (the process of checking WHO you are and WHAT you're allowed to do) |
| **Access control** | Based on granted privileges (via GRANT/REVOKE, [[Chapter 07 - SQL Data Definition|Ch.7]]) — determines what specific operations (SELECT, INSERT, etc.) a user can perform on specific objects |
| **Views** | Restrict a user's access to only the rows/columns relevant to them ([[Chapter 04 - The Relational Model|Ch.4]]) — a security mechanism, not just a convenience one |
| **Backup and recovery** | Protects against LOSS of data (from failure or accidental deletion) — ties directly to [[Chapter 22 - Transaction Management|Ch.22's]] recovery techniques |
| **Integrity** | Constraints (entity/referential/general, [[Chapter 04 - The Relational Model|Ch.4]]) prevent data from becoming invalid in the first place |
| **Encryption** | Encoding data so it's unreadable without the correct decryption key — protects data both in storage and in transit |
| **RAID (Redundant Array of Independent Disks)** | Hardware-level redundancy across multiple physical disks, protecting against DISK failure specifically (a different threat than the software-level controls above) |

> **Exam trap**: authorization and access control are related but distinct — authorization is the broader granting/verification of rights; access control is the mechanism that actually ENFORCES those rights at each operation.

---

## 4. Discretionary vs. mandatory access control (conceptual distinction, brief)
- **Discretionary Access Control (DAC)**: the OWNER of an object decides who gets which privileges (this is what SQL's GRANT/REVOKE implements) — flexible, but relies on individual owners making good decisions.
- **Mandatory Access Control (MAC)**: access is governed by a SYSTEM-WIDE policy (e.g. security classification levels: Top Secret > Secret > Confidential > Unclassified) that individual users/owners cannot override — used in high-security government/military-style systems, much more rigid than DAC.

---

## 5. RAID levels (brief overview, know the general idea more than every level's exact numbers)
RAID improves either performance, fault tolerance, or both, by spreading/duplicating data across multiple physical disks. Common levels: **RAID 0** (striping — improves performance, but NO redundancy, actually increases failure risk since ANY disk failing loses everything); **RAID 1** (mirroring — full duplication, high redundancy, but doubles storage cost); **RAID 5** (striping WITH distributed parity — a good balance of performance, redundancy, and storage efficiency, tolerates a single disk failure).

> **Exam trap**: RAID 0 is often mistakenly assumed to improve reliability because it "sounds like" a redundancy technology — it does NOT; it improves speed only and is actually MORE fragile (any one disk failing loses all data across the array).

---

## 6. Data Administration vs. Database Administration (revisits Ch.1's DA/DBA roles, in more depth here)
| | Data Administration (DA) | Database Administration (DBA) |
|---|---|---|
| Focus | The data RESOURCE — policy, planning, standards | The DATABASE(S) — technical implementation and operation |
| Nature of role | More managerial/strategic | More technical/hands-on |
| Typical tasks | Data policy, standards, planning, liaison with the organization | Physical implementation, security enforcement, performance monitoring, backup/recovery execution |

---

## 7. Quick self-test
1. Define database security, and list the 5 categories of threat.
2. Distinguish accidental threats from deliberate threats, and explain why the distinction matters for choosing countermeasures.
3. List the 7 computer-based security controls, and briefly state what each protects against.
4. Distinguish authorization from access control.
5. Distinguish Discretionary Access Control from Mandatory Access Control.
6. Why does RAID 0 NOT improve reliability, despite being a "RAID" technology?
7. Distinguish the DA role from the DBA role in terms of focus and typical tasks.
