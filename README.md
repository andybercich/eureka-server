# Eureka Server

Service discovery server for the Java microservices architecture.

The Eureka Server provides a centralized service registry where microservices register themselves and discover other services by service name.

## Purpose

The service provides service discovery for the microservices platform.

Instead of requiring services to communicate using fixed host addresses, services register with Eureka and can discover other services through their registered service names.

## Responsibilities

* Maintain the service registry.
* Allow microservices to register themselves.
* Allow services to discover registered services.
* Provide service availability information to the microservices architecture.
* Support dynamic service-to-service communication.

## Architecture

Eureka is part of the internal infrastructure of the platform.

```text
                         ┌─────────────────┐
                         │   API Gateway   │
                         └────────┬────────┘
                                  │
                                  │ Service discovery
                                  │
                         ┌────────▼────────┐
                         │ Eureka Server   │
                         │     :8761       │
                         └────────┬────────┘
                                  │
             ┌────────────────────┼────────────────────┐
             │                    │                    │
             ▼                    ▼                    ▼
      ┌─────────────┐      ┌─────────────┐      ┌─────────────┐
      │ Auth Service│      │Order Service│      │Product      │
      │             │      │             │      │Service      │
      └─────────────┘      └─────────────┘      └─────────────┘
                                  │
                                  │
                                  ▼
                           ┌─────────────┐
                           │  Analytics  │
                           │   Service   │
                           └─────────────┘
```

The API Gateway and microservices use Eureka for service discovery within the application network.

## Service Registration

Each microservice configured as a Eureka client registers itself with the Eureka Server.

Services are identified by their Eureka service name rather than relying exclusively on fixed container addresses.

This allows the infrastructure to resolve services dynamically within the deployment environment.

## Port

The Eureka Server runs on:

```text
8761
```

The Eureka dashboard and registry are intended for internal infrastructure use and should not be exposed directly to the public internet.

The API Gateway is the public HTTP entry point of the application.

## Technology Stack

* Java 17
* Spring Boot
* Spring Cloud Netflix Eureka Server
* Gradle
* Docker

## Configuration

The Eureka Server is configured as a Eureka Server application using Spring Cloud Netflix Eureka.

Microservices connect to the Eureka registry using the configured Eureka server URL.

In Docker deployments, services communicate using the internal Docker network and the Eureka service name instead of `localhost`.

For example:

```text
http://eureka:8761/eureka
```

The actual URL can be provided through environment-specific configuration.

## Docker Deployment

The service is containerized and can run together with the rest of the microservices infrastructure.

A typical deployment contains:

```text
API Gateway
Auth Service
Order Service
Product Service
Analytics Service
Eureka Server
Kafka
Redis
MySQL
N8N
```

Eureka is part of the internal infrastructure and does not need to be publicly exposed.

## Failure Considerations

Eureka is a service-discovery component, so its availability affects the ability of services to discover registered instances.

The application architecture separates service discovery from the business logic of the individual microservices.

Eureka does not store business data and does not process orders, products, inventory, or analytics.

## Role in the Project

Eureka Server provides the service-discovery layer of the microservices architecture.

It works together with the API Gateway and the individual Spring Boot services to enable service-to-service communication without hardcoding the location of every service.

## Project Status

The Eureka Server is part of the deployed microservices infrastructure and is required by the services that use Eureka-based service discovery.

