# my-k8s-deployment

## Project Overview
This is a Kubernetes deployment, automates project building using helm. 
Helm is the industry-standard package for Kubernetes. 
Why? Because it streamlines the process of application deployment,configuration and maintenance in cloud environments.
This project illustrates my ability to design and implement modern DevOps & Cloud infrastructure patterns, with emphasis on repeatability, scalability and configuration management.

## Project structure

my-k8s-deployment/

├── Chart.yaml              # Chart metadata and dependencies

├── values.yaml             # Centralized configuration values

├── .helmignore             # Files excluded from packaging

└── templates/              # Kubernetes manifest templates

    ├── _helpers.tpl        # Shared template functions and definitions
    
    ├── configmap.yaml      # Application configuration management
    
    ├── deployment.yaml     # Main workload controller with rollout strategy
    
    ├── hpa.yaml            # Horizontal Pod Autoscaler for dynamic scaling
    
    ├── ingress.yaml        # External traffic routing and TLS termination
    
    ├── networkpolicy.yaml  # Zero-trust network security policies
    
    └── service.yaml        # Internal service discovery and load balancing

## Technical Architecture & Design Philosophy
The chart is designed in a modular and extensible pattern, and it separates configuration (values.yaml) from implementation i.e., templates/.
This adheres to the best practices of creating a helm library, that makes it convenient and easy to customise specific services.

## Core Design Principles:
It enforces security by default by supporting non-root containers, explicit resource quotas, and secret management integration.
Designed for Operational Readiness to support monitoring (prometheus metrics and liveness/readiness probes)
Its environment parity capability ensures that helm values utility overrides the possibility of identical deployment across environments.

## Template Implementation

#### 1. Deployment Template (deployment.yaml)
The main workload controller that defines how the  application runs:

Rolling Update Strategy for  Zero-downtime deployments 

Configurable liveness and readiness probes for health monitor

Resource Management for CPU/memory requests and limits scheduling

It posseses Security Context that supports non-root execution, read-only root filesystems

#### 2. Service Template (service.yaml)
Internal networking and load balancing:

Service Types Support ClusterIP, NodePort, and LoadBalancer

Session Affinity Configures client IP-based session persistence

Port Management to support Multiple port definitions for complex applications

Selector Labels for Dynamic pod selection

#### 3. Horizontal Pod Autoscaler (hpa.yaml)
Automatic scaling based on metrics:

Enables CPU/Memory Scaling based on resource utilization thresholds

Integrates with Prometheus adapter

Configurable Boundaries: Minimum and maximum replica counts

It Prevents rapid scaling fluctuations

#### 4. Ingress Template (ingress.yaml)
External access and traffic management:

It supports Host-Based Routing with path-based rules

Support multipule ingress classes (nginx, traefik and AWS ALB)

SSL/TLS termination with certificate management


#### 5. ConfigMap Template (configmap.yaml)
Externalized configuration management:

Separates configuration from container images

Different values per environment

#### 6. NetworkPolicy Template (networkpolicy.yaml)

With the Ingress/Egress Rules, Fine-grained control over pod communications is achieved.

Namespace Isolation prevents cross-namespace traffic

Pod Selector Rules permits traffic only between specific pod groups

Denies all traffic unless explicitly allowed

#### 7. Template Helpers (_helpers.tpl)
Reusable template functions and conventions:

Ensures consistent label generation across all resources

Standardizes naming utilizing release names and chart names

Supports Reusable selectors for service-to-pod associations

## Key Features & Benefits

#### Security
Network policies ensure to implement zero-trust principles

Non-root container execution is supported by default

Supports Secret management integration 

#### Scalability
Horizontal Pod Autoscaler for automatic scaling

Resource requests/limits for predictable performance

Readiness probes for smooth scaling operations


#### Lifecycle Management
Rolling updates with configurable strategies to ensure zero downtime

Health checks (liveness/readiness) for reliability

Startup probes for slow-initializing applications

#### Operational Excellence
Designed for Structured logging and monitoring.

Standardizes resource naming and labeling

Environment-specific configuration management

Ensures Backup and disaster recovery


#### Best Practices Demonstrated

Immutable Infrastructure i.e, Container tags are versioned and unchangeable

With this setup Everything is defined in YAML, applied declaratively (Declarative Configuration)

Prevents configuration drift since Helm manages releases and prevents manual changes

Resources are optimized through requests/limits for cost control and performance

Security Hardening by implementing Network policies, non-root users, utilizing minimal images

Designed  for metrics, logs, and tracing for observability.

