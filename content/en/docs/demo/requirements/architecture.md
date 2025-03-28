---
title: Architecture Requirements
linkTitle: Architecture
aliases: [architecture_requirements]
cSpell:ignore: dockerstatsreceiver
---

## Summary

The OpenTelemetry Community Demo application is intended to be a showcase for
OpenTelemetry API, SDK, and tools in a production-lite cloud native application.
The overall goal of this application is not only to provide a canonical 'demo'
of OpenTelemetry components, but also to act as a framework for further
customization by end-users, vendors, and other stakeholders.

### Requirements

- [Application Requirements](../application/)
- [OpenTelemetry Requirements](../opentelemetry/)
- [System Requirements](../system/)

### Application Goals

- Provide developers with a robust sample application they can use in learning
  OpenTelemetry instrumentation.
- Provide observability vendors with a single, well-supported, demo platform
  that they can further customize (or simply use OOB).
- Provide the OpenTelemetry community with a living artifact that demonstrates
  the features and capabilities of OTel APIs, SDKs, and tools.
- Provide OpenTelemetry maintainers and WGs a platform to demonstrate new
  features/concepts 'in the wild'.

The following is a general description of the logical components of the demo
application.

## Main Application

The bulk of the demo app is a self-contained microservice-based application that
does some useful 'real-world' work, such as an eCommerce site. This application
is composed of multiple services that communicate with each other over gRPC and
HTTP and runs on Kubernetes (or Docker, locally).

Each service shall be instrumented with OpenTelemetry for traces, metrics, and
logs (as applicable/available).

Each service should be interchangeable with a service that performs the same
business logic, implementing the same gRPC endpoints, but written in a different
language/implementation.

Each service should be able to communicate with a feature flag service in order
to enable/disable faults that can be used to illustrate how telemetry helps
solve problems in distributed applications.

## Feature Flag Component

Feature flagging is a crucial part of cloud native application development. The
demo uses OpenFeature, a CNCF incubating project, to manage feature flags.

Feature flags can be set through the flagd configurator user interface.

## Orchestration and Deployment

All services run on Kubernetes. The OpenTelemetry Collector should be deployed
via the OpenTelemetry Operator, and run in a sidecar + gateway mode. Telemetry
from each pod should be routed from agents to a gateway, and the gateway should
export telemetry by default to an open source trace + metrics visualizer.

For local/non-Kubernetes deployment, the Collector should be deployed via
compose file and monitor not only traces/metrics from applications, but also the
docker containers via `dockerstatsreceiver`.

A design goal of this project is to include a CI/CD pipeline for self-deployment
into cloud environments. This could be skipped for local development.
