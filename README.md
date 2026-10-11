
# OT Security Evidence Repository

## Student Information
- **Student Name:** Muhammad Mukhtar Bello
- **Registration Number:** 17371
- **Course:** Intro to Critical Infrastructure for IT Learners
- **Lab:** OT Security Evidence and Validation
- **Institution:** International Cybersecurity and Digital Forensics Academy

## Environment Details
- **Host OS:** [Your host operating system]
- **PLC VM:** [Actual VM name and IP address]
- **SCADA VM:** [Actual VM name and IP address]
- **Virtualization:** Oracle VirtualBox
- **Capture Interface:** Loopback (`lo`), where applicable
- **Course Commit Hash:** [Required course commit hash]

## Repository Purpose
This repository contains the evidence collected during the OT security
lab, including preflight checks, network captures, Modbus register
definitions, guard-rule analysis, scenario validation, and the final report.

## Evidence Structure
- `preflight/` — Safety declaration and environment verification.
- `captures/` — Modbus packet capture and Wireshark analysis.
- `data_model/` — Modbus register definitions.
- `guard_analysis/` — Guard decision table and sample events.
- `validation/` — Evidence for each validation scenario.
- `report/` — Final lab report.

## Safety and Scope
All testing must remain within the authorized, isolated lab environment.
No testing against production systems or unauthorized networks is permitted.

## Evidence Status
Evidence will be added as each lab task is completed and verified.
