# Drone Platform Monitoring Server

---
## How It Works
The monitoring server runs as a separate(local) service and connects to
Redis and MySQL instances <br>that are already running on the main [Drone Platform Server](https://github.com/seyun4047/drone-platform-server).

This system uses a Redis ZSET-based heartbeat monitoring mechanism
<br>to automatically detect and disconnect drones that stop sending heartbeats.

---
## What It Monitors
The primary role of this service is to detect abnormal drone states, including:
- Drones that have been inactive for a specified period
- Drones that stop sending heartbeat signals
- Drones that fail to send status updates

---
## How It Helps
By identifying and handling these cases, the Monitoring Server helps:
- Prevent unnecessary server resource consumption
- Reduce database load
- Maintain overall platform stability

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

---
## Monitoring Flow
The monitoring process follows the flow below: 
|Monitoring Server|
|---|
|<img width="450" alt="Untitled diagram-2026-02-11-173920" src="https://github.com/user-attachments/assets/adbbeee5-7544-46c0-a276-0a04aae3e303" />|

---
