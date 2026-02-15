# drone-platform-docs

This repository contains **technical documentation** for each component of the
**Manufacturer-Independent Drone Platform**.

Each document provides detailed explanations of the system design, architecture,
and operational logic for individual services.


| Component            | Docs Name | Description                                              | Documentation |
|----------------------|-------------|----------------------------------------------------------|---------------|
| Platform|Overview| Platform Overview| [Docs](https://github.com/seyun4047/drone-platform-docs/blob/main/components/platform/overview.md) |
| Server               | Server        | Core drone platform server (API, authentication, telemetry) | [Docs](https://github.com/seyun4047/drone-platform-docs/blob/main/components/server/server.md) |
| Server               | DB Guide    | Database schema, relationships, and data management guide | [Docs](https://github.com/seyun4047/drone-platform-docs/blob/main/components/server/DB_GUIDE.md) |
| Monitoring Server    | Monitoring-Server | Real-time drone health and connection monitoring service  | [Docs](https://github.com/seyun4047/drone-platform-docs/blob/main/components/monitoring-server/monitoring-server.md) |
| Drone Data Tester    | Trans-Tester     | Test client for drone telemetry and data simulation       | [Docs](https://github.com/seyun4047/drone-platform-docs/blob/main/components/trans-tester/trans-tester.md) |
| Drone Client         | Client     | Drone data collection, transmission, and analysis         | [Docs](https://github.com/seyun4047/drone-platform-docs/blob/main/components/client/client.md) |
| Dashboard         | Dashboard     | Drone platform's front-end        | [Docs](https://github.com/seyun4047/drone-platform-docs/blob/main/components/dashboard/dashboard.md) |

---

## API's
### Drone Authentication & Data Transmission
| Method | Endpoint           | Description                              |
| ------ | ------------------ | ---------------------------------------- |
| POST   | `/auth/connect`    | Connect drone, receive token if approved |
| POST   | `/auth/update`     | Update/refresh token                     |
| POST   | `/auth/disconnect` | Disconnect drone                         |
| POST   | `/api/telemetry`                          | Receive telemetry data from drone              |

### Dashboard User
| Method | Endpoint              | Description   |
| ------ | --------------------- | ------------- |
| POST   | `/dashboard/register` | Register user |
| POST   | `/dashboard/login`    | Login user    |

### Dashboard Telemetry & Event
| Method | Endpoint                                  | Description                         |
| ------ | ----------------------------------------- | ----------------------------------- |
| GET    | `/api/dashboard/drone/telemetry/{serial}` | Get latest telemetry data for drone |
| GET    | `/api/dashboard/drone/event/{serial}`     | Get latest event data for drone     |
| GET    | `/api/dashboard/alive-drones`             | Get list of alive drones            |
