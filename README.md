# AquaWell Sysadmin Toolkit (IT 123 SAM Final Project)

Automated database lifecycle management, compressed `.tar.gz` archiving, and disaster recovery toolkit for **PostgreSQL 18** on **Ubuntu Linux (WSL2)**, built for **IT 123: System Administration and Maintenance (Option 1: Sysadmin Toolkit)**.

---

## 1. Project Overview

The **AquaWell Sysadmin Toolkit** automates end-to-end administration for the `aquawell` relational database (`127.0.0.1:5432`), replacing manual SQL and shell commands with modular, POSIX-compliant Bash scripts:

- **Automated Provisioning (`setup.sh`):** Verifies the PostgreSQL daemon, creates the `aquawell` database and least-privilege `aquawell_app` role, imports 16 application tables (`aquawell_schema.sql`) and synthetic test data (`seed_data.sql`), applies schema/sequence grants, and runs live verification checks.
- **Live Health Inspection (`view.sh`):** Connects as `aquawell_app` to verify server port bindings, list all 16 public tables, and inspect live order and inventory records.
- **Compressed TAR Archiving (`backup.sh`):** Exports a custom-format PostgreSQL dump (`pg_dump -F c`) to `/tmp`, compresses it into a timestamped `.tar.gz` archive inside `backups/`, and removes temporary staging files.
- **Automated Disaster Recovery (`restore.sh`):** Extracts the newest (or user-specified) `.tar.gz` archive into a PID-isolated `/tmp` folder, rebuilds the `aquawell` database, restores schema and records via `pg_restore --no-owner`, reapplies permissions, and verifies live tables.

---

## 2. Directory Structure

```text
aquawell/
├── README.md                 # Project overview, operational runbook, and AI disclosure
├── aquawell_schema.sql       # DDL definitions for 16 application tables
├── seed_data.sql             # Synthetic test records (orders, inventory, users)
├── backups/                  # Timestamped compressed .tar.gz backup archives
├── man/
│   └── aquawell.1            # UNIX roff manual page (open via: man ./aquawell.1)
├── samples/
│   └── sample_output.txt     # Verified --help and test database inspection outputs
└── scripts/
    ├── setup.sh              # Provisions DB, aquawell_app role, schemas, and seed data
    ├── view.sh               # Read-only live server and table inspection
    ├── backup.sh             # Exports custom dump and packages into backups/*.tar.gz
    └── restore.sh            # Extracts .tar.gz, rebuilds DB, and verifies recovery
```

---

## 3. Quick Start & Operational Runbook

### Step 1: Provision the Database & Application Role
```bash
~/aquawell/scripts/setup.sh
```

### Step 2: Run Live Server & Table Health Checks
```bash
~/aquawell/scripts/view.sh
```

### Step 3: Create a Compressed `.tar.gz` Backup
```bash
~/aquawell/scripts/backup.sh
```

### Step 4: Execute Disaster Recovery from Backup
```bash
# Automatically restores the newest archive in ~/aquawell/backups/
~/aquawell/scripts/restore.sh

# Or restore a specific archive path:
~/aquawell/scripts/restore.sh ~/aquawell/backups/aquawell_backup_20261127_122919.tar.gz
```

### Step 5: View Built-In CLI Usage & UNIX Manual Page
```bash
for s in ~/aquawell/scripts/*.sh; do "$s" --help; done
cd ~/aquawell/man && man ./aquawell.1
```

### Step 6: Package Full Submission Archive
```bash
cd ~ && tar -czvf aquawell_sam_project.tar.gz aquawell/
```

---

## 4. Security & Privacy Compliance

- **Environment-Overridable Credentials:** Database authentication defaults to a local test password (`DB_PASS:-12345`) and can be overridden via the `DB_PASS` environment variable (`DB_PASS="custom_test_pass" ./scripts/setup.sh`). No production secrets or API keys are hardcoded.
- **Synthetic Test Data Only:** All sample records in `seed_data.sql` and `samples/sample_output.txt` use synthetic test data (`Maria Santos`, `ORD-2026-0001`, sample water refill inventory) with zero real customer data.

---

## 5. Sources & AI Usage Disclosure

### Official Documentation & Course References
- **PostgreSQL 18 Documentation:** Referenced `psql`, `pg_dump -F c`, `pg_restore --no-owner`, `DO` role-creation blocks, and `GRANT` syntax.
- **GNU Coreutils & GNU `tar` Manual:** Referenced gzip archive compression (`tar -czf`), non-destructive inspection (`tar -tzvf`), extraction (`tar -xzvf`), and POSIX file permissions (`chmod +x`).
- **IT 123 Course Modules:** Applied standards across Modules 07, 08, 09, 10, 12, 13, 15, and 17.

### AI Assistance & Verification
- **Files & Line Ranges with AI-Assisted Content:**
  - `scripts/setup.sh` (lines 1 to 46)
  - `scripts/view.sh` (lines 1 to 15)
  - `scripts/backup.sh` (lines 1 to 27)
  - `scripts/restore.sh` (lines 1 to 51)
  - `man/aquawell.1` (lines 1 to 33)
  - `README.md` (lines 1 to 118)
  - `AquaWell_SAM_Project_Documentation_v4.docx` (Sections 01 to 08)
- **How AI Was Used:** Google Gemini assisted with structuring the four Bash scripts, adding `--help` flags, troubleshooting `.dump` vs. `.tar.gz` archive mismatches, formatting the UNIX roff manual page, and organizing project documentation.
- **Human Testing & Verification:** Every script, flag, manual page, and archive command was manually executed and verified in Ubuntu Linux (WSL2) against PostgreSQL 18 (`ls -lhR`, `--help`, `man ./aquawell.1`, `tar -tzvf`, and full `setup.sh`, `view.sh`, `backup.sh`, and `restore.sh` runs).