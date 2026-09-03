# Raheem — Semester Study Vault (CS-2, 2026/2027 Sem I)

Everything for turning each course unit's textbook into exam-ready study material, one chapter at a time.

## Structure
```
Obsidian-Vault/           <- open this FOLDER as an Obsidian vault
  AI (CSC2114)/
    00 - Course Map.md
    Chapter 01 - Introduction.md
    (more chapters land here as we go)
Anki/                     <- File > Import in Anki, one .txt per chapter
  AI_CSC2114_Ch01_Introduction.txt
Practice-Questions/       <- closed-book self-test, answers included
  AI_CSC2114_Ch01_Questions.md
Codex/
  mini_codex.html         <- your dashboard, patched to persist CGPA via localStorage
                              and updated with the real CS-2 timetable
```

## Workflow per chapter (repeat for every course unit)
1. I read the assigned chapter from your reference book for that unit.
2. I write the Obsidian note in student-persona, exam-focused style (not a textbook copy).
3. I generate a matching Anki `.txt` deck (tab-separated, `#deck:` header pre-set — just double-click import in Anki or use File > Import).
4. I generate practice questions with a full answer key.
5. You commit: `git add -A && git commit -m "add <course> ch<N>"`.

## Setting this up on GitHub (do this once)
```bash
# from inside this folder, after unzipping:
git remote add origin https://github.com/<your-username>/<repo-name>.git
git branch -M main
git push -u origin main
```
Create the empty repo on GitHub first (no README/license, so it doesn't conflict with this one).

## Codex note
`Codex/mini_codex.html` was previously reading/writing state via `window.storage`, which only exists inside a Claude artifact sandbox — that's why your CGPA wasn't surviving reloads once deployed on Vercel. It now uses `localStorage` directly, so it persists in any normal browser. Replace the corresponding file in your `raheem-codex` repo with this one (or diff it in) and redeploy.

## Status
- [x] AI (CSC2114) — Ch.1 Introduction
- [x] AI (CSC2114) — Ch.2 Intelligent Agents (added back — lecturer resumed teaching it)
- [x] AI (CSC2114) — Ch.19 Machine Learning
- [x] AI (CSC2114) — Ch.3 Search
- [x] AI (CSC2114) — Ch.4 Search in Complex Environments (added back — lecturer resumed teaching it)
- [x] AI (CSC2114) — Ch.17 MDPs
- [x] AI (CSC2114) — Ch.5 Adversarial Search / Games
- [x] AI (CSC2114) — Ch.6 CSPs
- [x] AI (CSC2114) — Ch.12-13 Bayesian Networks — **AI course now fully covered per the syllabus map**
- [x] Embedded and Real-time Systems (CSC 2118) — **fully complete: Intro, ESP32-S3, FreeRTOS (3 parts), ESP-IDF setup, I/O & sensors, wireless, serial comms, security, plus the C-foundations prerequisite**
- [x] Discrete Mathematics (CSC 2105) — **fully complete: Logic/Proofs, Sets/Functions, Algorithms, Number Theory, Induction/Recursion, Counting, Recurrence Relations, Relations, Boolean Algebra (9 chapters)**
- [ ] Computer Networks (BSE 2106)
- [x] Database Management Systems (CSC 2107) — **13 core chapters complete**: Intro, Environment/Architecture, Relational Model, Algebra/Calculus, SQL DML/DDL, Design Lifecycle, ER/EER Modeling, Normalization/Advanced Normalization, Transactions, Security
