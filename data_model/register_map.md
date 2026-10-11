# Activity 02 - Modbus/TCP Data Model & Normal Operations

## Source Code References
- **Plant Data Model:** `plant.py` (Lines 16-23)
- **Guard Validation Rules:** `guard.py` (Lines 24-35 & 42-45)

## Modbus Register Map

| Process Variable | Register Address | Scale / Unit | Normal Operating Range | Alarm Threshold | Guard Max Step | Guard Drift Limit (3-Step Window) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Temperature** | `0` (40001) | `/10` (°C) | 60.0 to 75.0 °C | N/A | 3.0 °C (30) | 2.0 °C (20) |
| **Pressure** | `1` (40002) | `/10` (bar) | 30.0 to 40.0 bar | >= 39.5 bar | 5.0 bar (50) | 2.0 bar (20) |
| **Flow Rate** | `2` (40003) | `/10` (Nm³/h) | 50.0 to 150.0 Nm³/h | N/A | 10.0 Nm³/h (100) | 4.0 Nm³/h (40) |
| **System Alarm** | `3` (40004) | None (Binary) | `0` (Normal) | `1` (Active) | N/A | N/A |

## Normal Transaction & Role Explanation

### Client and Server Roles
- **Modbus Server (Slave):** The Plant simulator (`plant.py` running on `127.0.0.1:5020`) acts as the server. It maintains the physical process state in its internal memory block and autonomously updates the values within normal operating bounds every 1.0 second.
- **Modbus Clients (Master):** The Direct Dashboard (`dashboard.py`) and Guarded Dashboard (`dashboard_guarded.py`) act as Modbus clients. They routinely send requests over the network to poll the server for the latest register values to update the UI.

### Normal Read Transaction (Baseline Correlation)
During normal operations, the dashboard client initiates a Modbus transaction to read the process telemetry:
1. **Request:** The client sends a packet using **Function Code 3 (Read Holding Registers)**, requesting a continuous block starting at **Address 0** for a **Quantity of 4** registers.
2. **Response:** The plant server replies with a data payload of integers (e.g., `[670, 340, 900, 0]`). 
3. **Translation:** The client receives these integers and divides the first three by the scale factor of 10. This matches the baseline observed on the dashboard: `67.0 °C` (Temperature), `34.0 bar` (Pressure), and `90.0 Nm³/h` (Flow Rate). The fourth register is evaluated as a boolean `0`, meaning the alarm is currently inactive. If pressure reaches 39.5 bar (`395`), the server automatically flips this register to `1`.
