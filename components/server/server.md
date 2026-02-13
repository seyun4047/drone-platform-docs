# Drone Platform Server

---
## Overview
The Drone Platform Server is the core backend service of the Manufacturer-Independent Drone Platform.<br>

It is responsible for managing the entire lifecycle of drone sessions, authentication, <br>telemetry processing, and high-volume request handling across the platform.<br>

This server acts as the central coordination layer between drones, data storage systems, and external services.

---

## Core Responsibilities

### 1. Drone Authentication & Session Management
The server manages secure and scalable drone sessions using:
- Authorized Drone Database (MySQL)<br>
Maintains the list of registered and approved drones.
- Drone Authentication Tokens (Redis)<br>
Issues and validates access tokens for authenticated drones.
- Drone Heartbeat Tracking (Redis)<br>
Tracks real-time connectivity status using time-indexed heartbeat data.

### 2. Telemetry & Event Processing
The server receives and processes telemetry and event data transmitted from [drone-client](https://github.com/seyun4047/drone-platform-client), including:
- Telemetry Data<br>
Processes real-time operational status data from active drones.
- Event Data<br>
Processes event data, such as Human detection and other mission-triggered activities.

### 3. Dashboard Authentication
The server handles user registration and approval for dashboard access:
- User Registration & Approval Workflow<br>
Users can register for dashboard access. Only approved users can log in.
- Access Control Enforcement<br>
Only authenticated and authorized users can access dashboard APIs, ensuring data integrity and privacy.

### 4. Dashboard API Access
The server provides secure, JWT-based access for the web dashboard and other frontend clients:
- JWT Token Generation & Verification<br>
Upon successful login, the server issues a JWT token which clients must include in API requests.<br>The server validates the token for all protected endpoints.
Secured Dashboard Endpoints<br>
- Fetching alive drones from Redis heartbeat data.
- Retrieving latest telemetry and event data for drones.

---
## Usage
### Local Build
```bash
# Export env
export $(cat .env | xargs)

# Build the project (Gradle)
./gradlew build

# Run locally with 'local' profile
./gradlew bootRun --args='--spring.profiles.active=local'
```
### Docker Build
```bash
# Build Images and Start (MySQL, Redis, App)
docker compose up --build

# Stop and remove containers
docker compose down
```
---
## Test-Drone Data
### Flow Test with Mock Drone Data
```bash
# Test
./gradlew test
```
### Flow Test with Real Drone Data
> If you want to test with real drone data, check it out here: [Drone Data Tester](https://github.com/seyun4047/drone-platform-trans-tester)   

## Test-Dashboard Auth
### Flow Test with Real User Data
```bash
# register
curl -X POST http://<your_url>/dashboard/register \
  -H "Content-Type: application/json" \
  -d '{
        "username": "testid",
        "password": "testpw"
      }'
# return {"status":status,"message":"message","data":{"id":"testid"}}%
```
```bash
# login
curl -X POST http://<your_url>/dashboard/login \
  -H "Content-Type: application/json" \
  -d '{
        "username": "testid",
        "password": "testpw"
      }'
# return {"status":status,"message":"message","data":{"token":"jwt"}}%
```
```bash
# Get Drone Data
 curl -i --location --request GET 'http://<your_url>/api/dashboard/drone/<telemetry_or_event> /<testid>' \
-H "Auth: Bearer <token>"
# return {"data":{},"updatedAt":currentTimeMillis}%
```
 
---
## DB GUIDE
### MYSQL DB USAGE GUIDE
>  If you want to know MySQL usage guide, check it out here: [DB GUIDE](https://github.com/seyun4047/drone-platform-docs/blob/main/components/server/DB_GUIDE.md)
---

## Dashboard(Front-End) Communication
<img height="900" alt="AWS Upload Presigned URL-2026-02-13-144904" src="https://github.com/user-attachments/assets/4e956658-5ef2-4c1d-972d-ea669aa09b67" />