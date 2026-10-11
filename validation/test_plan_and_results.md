# Activity 06 - Controlled Security Validation

## Test Plan & Execution Log

| Scenario | Target Port | Expected Result | Observed Result | Recovery Verified |
| :--- | :--- | :--- | :--- | :--- |
| **1. Out-of-Range Attack** (`attack_hold.py`) | Direct `5020` | Process state forced out of bounds; alarm triggers. | 53/56 writes bypassed safe range. Peak pressure reached 95.0 bar. | Yes |
| | Guarded `5021` | Guard blocks writes (Absolute Range Check); state remains normal. | 0/53 writes bypassed safe range. Peak pressure stayed at 38.0 bar. Attack contained. | Yes |
| **2. False Data Injection** (`false_data_attack.py`) | Direct `5020` | Process state swings rapidly. | Unsafe readings for all variables. Peak Temp: 110.0C, Pressure: 55.0bar, Flow: 200.0Nm3/h. | Yes |
| | Guarded `5021` | Guard drops packets (Max Step-Size Check). | Peak values stayed within safe range (Temp: 74.6C, Pressure: 40.0bar, Flow: 150.0Nm3/h). Attack contained. | Yes |
| **3. Slow Drift Attack** (`slow_drift_attack.py`) | Direct `5020` | State slowly creeps into danger zone. | Attack requested values up to 51.5 bar; observed reads peaked at 40.0 bar due to plant clamping. | Yes |
| | Guarded `5021` | Guard blocks cumulative change (Rolling Drift Check). | Attack contained. Guard successfully identified and blocked cumulative drift exceeding the 2.0 bar limit. | Yes |

## Log Evidence (`events.jsonl`)
```jsonl
{"timestamp": "2026-10-11 14:12:08", "reason": "50.0 bar outside safe range 30.0-40.0 bar; step of 10.1 bar exceeds max 5.0 bar; cumulative drift of 10.7 bar over last 3 accepted writes exceeds 2.0 bar limit"}
{"timestamp": "2026-10-11 14:12:10", "reason": "51.0 bar outside safe range 30.0-40.0 bar; step of 11.1 bar exceeds max 5.0 bar; cumulative drift of 11.1 bar over last 3 accepted writes exceeds 2.0 bar limit"}
{"timestamp": "2026-10-11 14:12:12", "reason": "52.0 bar outside safe range 30.0-40.0 bar; step of 12.0 bar exceeds max 5.0 bar; cumulative drift of 12.1 bar over last 3 accepted writes exceeds 2.0 bar limit"}
{"timestamp": "2026-10-11 14:12:44", "reason": "50.4 bar outside safe range 30.0-40.0 bar; step of 11.3 bar exceeds max 5.0 bar; cumulative drift of 11.4 bar over last 3 accepted writes exceeds 2.0 bar limit"}
{"timestamp": "2026-10-11 14:12:46", "reason": "51.4 bar outside safe range 30.0-40.0 bar; step of 12.2 bar exceeds max 5.0 bar; cumulative drift of 12.3 bar over last 3 accepted writes exceeds 2.0 bar limit"}
