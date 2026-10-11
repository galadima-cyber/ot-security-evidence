# Scope Card & Safety Boundary

| Parameter | Authorized Value |
| :--- | :--- |
| **VM Environment** | Isolated Ubuntu Guest |
| **Target Host** | `127.0.0.1` (Loopback interface `lo` only) |
| **Permitted Ports** | `5020` (Plant), `5021` (Guard), `8080` (Direct Dash), `8081` (Guarded Dash) |
| **Prohibited Actions**| External scanning, port forwarding, bridged access, modifying script parameters |
| **Stop Condition** | Any packet leaving loopback, unexpected non-127.0.0.1 target, or VM instability |
| **Recovery Plan** | Stop processes via `Ctrl+C`, verify ports with `ss -tulpn`, revert to snapshot if needed |
