# Drone Platform Mornitoring Server
---
## How It Works
The monitoring server runs locally and connects to
  Redis and MySQL instances <br>that are already running on the main [Drone Platform Server](https://github.com/seyun4047/drone-platform-server).

## What Does It Monitor
Its primary role is to detect abnormal drones, including:
- Drones that have been inactive for a certain period of time
- Drones that are not responding to heartbeat or status updates

By identifying and handling these cases,
<br>the monitoring server helps prevent server overload and maintains the overall stability of the Drone Platform.

---

## Usage
```bash
# Load Environment Variables
set -a
source dev.env
set +a
```
```bash
# Build
./gradlew build
```
```bash
# Run
./gradlew bootRun
```

## Monitoring Flow
The monitoring process follows the flow below: 
|Monitoring Server|
|---|
|<img src="https://github.com/user-attachments/assets/592adb6b-9066-47ac-8f9d-d5117492a6af" width="450"/> |
