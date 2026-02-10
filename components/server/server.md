# Drone Platform Server
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
## Test
### Flow Test with Mock Data
```bash
# Test
./gradlew test
```
### Flow Test with Real Data
> If you want to test with real drone data, check it out here: [Drone Data Tester](https://github.com/seyun4047/drone-platform-trans-tester)   

---
## DB QUIDE
### MYSQL DB USAGE QUIDE
>  If you want to know MySQL usage guide, check it out here: [DB GUIDE](https://github.com/seyun4047/drone-platform-docs/blob/main/components/server/DB_GUIDE.md)
---
