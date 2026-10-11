# ACICP302ICS - OT Security Simulation Lab Final Report

**Student Name:** Muhammad Mukhtar Bello
**Date Completed:** 11 October 2026 (WAT)
**VM Environment:** Isolated Ubuntu Guest (Clean Snapshot Restored)
**Course Source Commit:** a66f9bcf253eb3fe31c68ad52794cd535421da8

## 1. Executive Summary
This repository contains the required evidence, packet captures, and analysis for the Modbus/TCP OT Security Simulation lab. The simulation was successfully executed within a strict loopback-only (`127.0.0.1`) safety boundary. The lab demonstrated the extreme vulnerability of unprotected direct-access Modbus OT assets and validated the effectiveness of a proxy guard implementing absolute range, step-size, and rolling-drift checks.

## 2. Evidence Index & Traceability
All laboratory activities have been documented and are indexed below:

* **Activity 00 (Preflight & Safety):** See `/preflight/` for the signed safety declaration, scope card, and source code loopback verification.
* **Activity 01 (Architecture & Baseline):** See `/captures/baseline_dashboards.png` for the normal steady-state telemetry.
* **Activity 02 (Data Model):** See `/data_model/register_map.md` for the reverse-engineered Modbus register map.
* **Activity 03 (Packet Analysis):** See `/captures/` for the loopback PCAP and the decoded Modbus MBAP/PDU payload breakdown.
* **Activity 04 & 05 (Exposure & Guard Analysis):** See `/guard_analysis/exposure_and_guard_analysis.md` for the local risk register and the defensive rule decision table.
* **Activity 06 (Controlled Validation):** See `/validation/test_plan_and_results.md` for the results of the Out-of-Range, False Data, and Slow Drift attacks against both the direct plant and the guarded proxy.

## 3. Verified Recovery Statement
I confirm that all lab processes have been terminated, local listening ports (`5020`, `5021`, `8080`, `8081`) have been verified closed, and the VM has been safely restored/cleaned as per the lab requirements.
