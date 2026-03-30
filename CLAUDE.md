# CLAUDE.md - Scheduler-API

## Project Overview

Scheduler-API is a lightweight Spring Boot backend service for the AMRIT platform that manages appointment scheduling for telemedicine. It handles specialist availability, time slot management, scheduling configurations, and van (mobile unit) assignments.

## Tech Stack

- Java 17, Spring Boot 3.2.2, Spring Data JPA
- MySQL 8.0 (via mysql-connector-j)
- Redis (session management)
- MapStruct (object mapping), Lombok
- Swagger/OpenAPI (springdoc-openapi)
- WAR packaging for Wildfly deployment
- JaCoCo (coverage), Checkstyle (style)

## Build & Run

```bash
mvn clean install -DENV_VAR=local          # Build
mvn spring-boot:run -DENV_VAR=local        # Run locally
mvn -B package --file pom.xml -P <profile> # Package WAR (dev, local, test, ci, uat)
mvn test                                    # Run tests
```

Environment is set via `-DENV_VAR=<env>` which selects `common_<env>.properties`.

## Package Structure

Base package: `com.iemr.tm`

| Package | Purpose |
|---------|---------|
| `controller/` | REST controllers (3 controllers) |
| `service/` | Business logic layer |
| `repo/` | JPA repositories |
| `data/` | JPA entity classes |
| `config/` | Spring configuration |
| `utils/` | Cross-cutting: Redis, HTTP clients, response wrappers, exception handling |

## Key Domains / Controllers

- **SchedulingController** - Time slot creation, day/month scheduling, appointment booking and cancellation
- **SpecialistController** - Specialist availability management and lookup
- **VanController** - Van (mobile unit) master data and assignments

## Architecture Notes

- This is a small, focused microservice with only 3 domain areas (schedule, specialist, van)
- Standard layered architecture: Controller -> Service -> Repository -> Entity
- Shares the `com.iemr.tm` base package with TM-API (telemedicine), as scheduling is a TM subdomain
- Redis used for session validation via `utils/redis/`
- Gateway utilities in `utils/gateway/` for email integration
- Response wrapper pattern in `utils/response/`
- Session object management in `utils/sessionObject/`
