# IoT Hub

IoT Hub is a distributed platform for registering IoT devices, ingesting telemetry in real time, evaluating business rules, and delivering system reactions such as alerts, webhooks, and audit events.

## Overview

The platform allows users to:

- register and manage their IoT devices
- define device metrics and telemetry configuration
- send telemetry over HTTP or MQTT
- store and query historical telemetry
- evaluate rules against incoming telemetry in real time
- trigger alerts and external actions such as webhook delivery

## Architecture

IoT Hub is evolving from a monolithic prototype into a microservices-based architecture.

The system is centered around an event-driven telemetry pipeline:

1. Telemetry is received through HTTP or MQTT
2. Incoming messages are published to Kafka
3. Telemetry is validated, deduplicated, and routed
4. Valid events are stored, streamed to clients, and processed by the rule engine
5. Rule matches produce alerts and action execution requests
6. Important events are recorded in the audit flow

### Main architectural blocks

#### Configuration block
- **Auth service** — authentication, authorization, identity, token management
- **Device registry service** — devices, metrics, bindings, ownership, telemetry configuration
- **Rule engine service** — rule storage and real-time rule evaluation

#### Telemetry pipeline block
- **Telemetry intake service** — HTTP and MQTT ingestion
- **Telemetry processing service** — validation, deduplication, routing
- **Telemetry storage service** — historical telemetry persistence and queries
- **Realtime service** — live telemetry delivery over WebSocket
- **Stream aggregator service** — rolling aggregates, derived metrics, real-time analytics

#### Outcome and observability block
- **Events service** — user-visible events triggered by rule evaluation and events lifecycle
- **Action dispatcher service** — external action execution, retries, delivery tracking
- **Audit service** — centralized audit trail for important system events

## Repository structure

This organization contains multiple repositories that represent different parts of the platform.

### Core services
- `auth-service`
- `device-registry-service`
- `telemetry-intake-service`
- `telemetry-processing-service`
- `telemetry-storage-service`
- `realtime-service`
- `rule-engine-service`
- `events-service`
- `action-dispatcher-service`
- `audit-service`

### Platform and shared repositories
- `iot-hub-platform` — production deployment and infrastructure orchestration
- `iot-hub-dev-workspace` — local multi-repo development workspace
- `python-service-libs` — shared Python infrastructure libraries

### Templates
- general microservice template repository
- Django microservice template repository
- Java microservice template repository

## Technology approach

The platform intentionally uses different technologies depending on service responsibilities.

### Django
Used where strong CRUD, admin capabilities, and relational domain modeling are valuable:
- auth
- device registry
- events
- audit

### FastAPI / lightweight Python services
Used where lightweight APIs, ingestion, or specialized service behavior are more important than a full Django stack:
- telemetry intake
- telemetry storage API
- realtime delivery
- action dispatching

### Plain Python workers
Used for stream-oriented internal processing where a full web framework is unnecessary:
- telemetry processing
- protocol adapters
- internal workers

### Java
Used for the rule engine, where controlled concurrency, rule execution, and enterprise-style processing are important.

### Scala
Planned for streaming analytics and aggregation workloads such as rolling metrics, windowed computations, and derived telemetry signals.

## Development model

IoT Hub is developed as a **multi-repository system**.

Each microservice has its own repository, CI pipeline, and deployment lifecycle.  
At the same time, the platform can be run together through a dedicated development workspace and orchestration repositories.

This setup is intended to support:

- independent service development
- clear ownership boundaries
- reusable shared libraries
- explicit cross-service contracts
- realistic microservice delivery workflows

## Engineering goals

This project emphasizes:

- clear bounded contexts
- event-driven service communication
- production-minded service design
- reusable shared infrastructure components
- observability and auditability
- scalable telemetry ingestion and processing
- clean repository and deployment structure

## Current focus

The platform is currently focused on:

- decomposing the monolith into independent microservices
- formalizing event contracts between services
- extracting shared infrastructure libraries
- building local and production orchestration layers
- preparing rule processing and stream aggregation services
- improving end-to-end integration and operational readiness
