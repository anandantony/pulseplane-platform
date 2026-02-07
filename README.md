# Pulseplane

**Pulseplane** is a production-grade feature flag and runtime configuration control plane,
designed for safe rollouts, kill switches, and predictable behavior in distributed systems.

It prioritizes **correctness, explicit defaults, and operational safety** while remaining
extensible for future experimentation and policy-driven control.

---

## Why Pulseplane?

Modern systems need to change behavior at runtime without redeploying services.
Pulseplane enables this by acting as a **control plane** that continuously propagates
configuration state to consuming services in a safe and observable way.

The name *Pulseplane* reflects this idea:
a control plane that keeps runtime behavior in sync through steady, reliable pulses.

> This project is designed as a reference-quality platform implementation,
> focusing on correctness, operability, and extensibility over feature breadth.

---

## Core Capabilities (V1)

- Feature flags (boolean, percentage-based, multivariate)
- Kill switches with sub-second propagation targets
- Runtime configuration management
- Deterministic, SDK-side evaluation
- Environment-scoped flags (dev / stage / prod)
- Explicit fallback defaults
- CI/CD-friendly bootstrap configuration export
- Permission-based access control
- Built-in observability hooks

---

## Architecture Overview

Pulseplane follows a **control plane / data plane** model:

- **Control Plane**
  - Flag definitions and versioning
  - Publishing and rollback
  - Permission enforcement
  - Distribution orchestration

- **Data Plane**
  - SDK-side evaluation
  - Local caching
  - Deterministic rollout decisions
  - Graceful degradation during outages

The platform guarantees **strong consistency for writes** and **eventual consistency for reads**.

---

## Distribution Model

V1 supports:

- HTTP polling (mandatory, correctness baseline)
- Redis Pub/Sub (optional, latency optimization)

Polling is always retained as a fallback to ensure correctness.

Kafka-based streaming is intentionally deferred to v2.

---

## Failure & Degradation Strategy

Pulseplane is designed to fail safely:

- SDKs use cached values when the control plane is unavailable
- Explicit defaults are used when cache is missing
- Default configurations can be exported from the platform UI
  and injected via CI/CD for cold starts and disaster recovery

No application should fail to start due to Pulseplane unavailability.

---

## Access Control

Pulseplane uses **permission-based access control**.

- The platform defines atomic permissions (e.g. `flag.publish`)
- Consumer applications define roles and assign permissions
- Authentication and identity are handled externally

This keeps the platform flexible while avoiding opinionated auth systems.

---

## What Pulseplane Is Not (V1)

- Not a secrets manager
- Not an experimentation platform
- Not a real-time evaluation engine
- Not a user-facing dashboard-first product

These are intentionally deferred.

---

## Documentation

Design and implementation details are documented in the `docs/` directory:

- [V1 Specification](docs/v1-spec.md)
- [Features](docs/features.md)
- [Architecture (HLD)](docs/architecture.md)
- [Low-Level Design (LLD)](docs/lld.md)
- [Access Control (RBAC)](docs/rbac.md)
- [CI/CD Bootstrap & Default Configs](docs/ci-cd-bootstrap.md)

---

## Status

This project is under active development and is intended as a
**reference-quality platform design** rather than a drop-in replacement
for existing feature flag services.

---

## License

Pulseplane is open-sourced under the [MIT License](LICENSE).
