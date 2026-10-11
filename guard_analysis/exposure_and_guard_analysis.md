# Activity 04 & 05 - Safe Discovery, Exposure Mapping, and Guard Analysis

## 1. Local Service Exposure & Risk Mapping (Activity 04)
The following local services were mapped using the `ss -tulpn` command. All services are strictly bound to the `127.0.0.1` loopback interface.

| Component Role | Protocol / Port | Process Significance | Risk of External Exposure |
| :--- | :--- | :--- | :--- |
| **Plant Simulator (Direct)** | Modbus/TCP (`5020`) | Contains raw process state and direct write access. | **CRITICAL:** Attackers could bypass the guard and manipulate process parameters directly, causing out-of-bounds physical states. |
| **Defensive Guard** | Modbus/TCP (`5021`) | Validates writes before forwarding to the plant. | **HIGH:** Attackers could launch denial-of-service (DoS) or attempt slow-drift attacks to manipulate state within validated thresholds. |
| **Direct Dashboard** | HTTP (`8080`) | Displays real-time physical plant data. | **MEDIUM:** Information disclosure of raw process telemetry. |
| **Guarded Dashboard** | HTTP (`8081`) | Displays validated process data. | **MEDIUM:** Information disclosure. |

## 2. Defensive Guard Decision Table (Activity 05)
Based on the inspection of `guard.py`, the defensive proxy implements the following rules on incoming Modbus writes.

| Guard Check / Rule | Condition Checked | Action if Violated | Audit Evidence / Log Event |
| :--- | :--- | :--- | :--- |
| **Absolute Range Check** | Is the requested value outside the safe physical bounds? (e.g., Temp < 60.0°C or > 75.0°C) | Block write, discard packet, do not forward to plant. | `BLOCKED write to reg X: absolute range check failed` |
| **Max Step-Size Check** | Is the difference between the current value and requested value larger than the max allowed sudden jump? (e.g., > 3.0°C) | Block write, discard packet, do not forward to plant. | `BLOCKED write to reg X: max step size exceeded` |
| **Rolling-Baseline Drift Check** | Over a 3-step historical window, does the cumulative accepted change exceed the safe drift limit? (e.g., > 2.0°C) | Block write, discard packet, do not forward to plant. | `BLOCKED write to reg X: rolling drift limit exceeded` |
| **Safe State Validation** | Are all requested parameters within bounds and limits? | Accept write, forward to direct plant (`5020`), update accepted history. | Write allowed; Dashboard reflects updated state. |

**Guard Limitations (Residual Risk):**
The guard lacks client authentication and authorization (any local client can connect). It only monitors individual variables independently and lacks multi-variable correlation (e.g., it does not check if temperature and pressure make sense together). Additionally, the guard can be entirely bypassed if an attacker discovers and routes traffic directly to port `5020`.
