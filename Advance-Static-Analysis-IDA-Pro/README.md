# 🔬 Advanced Static Analysis using IDA Pro

![Malware Analysis](https://img.shields.io/badge/Topic-Malware%20Analysis-red?style=for-the-badge)
![Tool](https://img.shields.io/badge/Tool-IDA%20Pro-blue?style=for-the-badge)
![Course](https://img.shields.io/badge/Course-Fundamentals%20of%20Malware%20Analysis-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)

---

## 📌 Overview

This project demonstrates **Advanced Static Analysis** of a Windows executable (`Task.exe`) using **IDA Pro** — a professional-grade disassembler and debugger widely used in reverse engineering and malware analysis. The goal was to analyze the binary **without executing it** and extract the hidden password through assembly-level inspection.

---

## 🎯 Objective

> Reverse engineer a password-protected executable using static analysis techniques in IDA Pro to discover the correct password embedded in the binary.

---

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| **IDA Pro** | Disassembly & static analysis |
| **ASCII Table** | Hex-to-character conversion |
| **Windows OS** | Running and testing the executable |

---

## 📋 Course Details

| Field | Details |
|-------|---------|
| **Course** | Fundamentals of Malware Analysis |
| **Instructor** | Mr. Jawad Hassan Nisar |
| **Student** | Sarmad Farooq |
| **Student ID** | 25i-7722 |
| **University** | National University of Computer and Emerging Sciences (FAST-NUCES) |

---

## 🔍 Analysis Walkthrough

### Step 1 — Load the Binary
- Downloaded `Task.exe` and placed it in a dedicated folder
- Opened the file in **IDA Pro** via right-click → *Open with IDA*

### Step 2 — Configure IDA Pro
- Selected **Portable Executable for AMD64 (PE)** as the file format
- Accepted DWARF debug info (Global names, Functions, Types, Calling conventions)

### Step 3 — Initial Execution (Pre-Analysis)
- Ran the executable to observe behavior
- Program displayed: *"Welcome Reverse Engineers!"*
- Prompted for a password → entered a random value (`123456`)
- Received **"Access Denied"** popup → confirmed password protection

### Step 4 — IDA Graph View
- Opened IDA's main **Graph View** to visualize the control flow
- Navigated to **View → Open Subviews → Strings**

### Step 5 — Strings Analysis
- Identified key strings in the `.rdata` section:
  - `"Greeting MAD & FMA"`
  - `"Welcome Reverse Engineers !"`
  - `"Enter the password:"`
  - `"Access Granted"` / `"Access Denied"`
  - `"Too Easy for you!"`
- Double-clicked **"Access Denied"** → navigated to its **XREF (cross-reference)**

### Step 6 — Control Flow Analysis
- Followed the XREF to identify the **branch condition** determining Access Granted vs Access Denied
- Located the **program entry point** (`main` function) in Graph View

### Step 7 — Assembly Instruction Analysis

```asm
; Program stores the password into Str2 using two MOV instructions:
mov    dword ptr [rbp+Str2],   37723958h   ; Bytes: 58 39 72 37 (little-endian)
mov    dword ptr [rbp+Str2+3], 505437h     ; Bytes: 37 54 50    (little-endian)
```

- Program reads user input via `scanf` into `Str1`
- Compares `Str1` with hardcoded `Str2` using `strcmp`

### Step 8 — Password Extraction

Applying **little-endian** byte reversal:

| MOV Instruction | Raw Hex | Little-Endian | ASCII |
|----------------|---------|----------------|-------|
| `[rbp+Str2]`   | `37 72 39 58` | `58 39 72 37` | `X 9 r 7` |
| `[rbp+Str2+3]` | `37 54 50`    | `37 54 50`    | `7 T P`   |

> ⚠️ Note: Second MOV starts at offset `+3`, overwriting index 3 of the first write.

**Final byte sequence:** `58 39 72 37 54 50`

**Decoded Password:** `X9r7TP`

### Step 9 — Verification
- Re-ran `Task.exe` and entered password: **`X9r7TP`**
- Result: ✅ **"Access Granted — Too Easy for you!"**

---

## 🧠 Key Concepts Demonstrated

- **Static Analysis** — analyzing a binary without executing it
- **Disassembly** — reading x86-64 assembly instructions
- **Little-Endian** byte ordering in memory
- **XREF (Cross-References)** — tracing string usage back to code
- **Control Flow Analysis** — understanding conditional branches in IDA Graph View
- **ASCII Decoding** — converting hex values to readable characters

---

## 📁 Project Files

```
Advance-Static-Analysis-IDA-Pro/
├── README.md
└── Advance Static Analysis by IDA Pro_report_by_SarmadFarooq.pdf
```

---

## ✅ Result

| Attempt | Password | Result |
|---------|----------|--------|
| Before Analysis | `123456` | ❌ Access Denied |
| After Static Analysis | `X9r7TP` | ✅ Access Granted |

---

## 👨‍💻 Author

**Sarmad Farooq** — MS Cybersecurity Student  
National University of Computer and Emerging Sciences (FAST-NUCES)  
Student ID: `25i-7722`

---

> *"The best way to understand a program is to read its assembly."*
