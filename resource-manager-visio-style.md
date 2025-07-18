
# Resource Manager - Visio-Style Flow Diagram

## System Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           USER INTERFACE LAYER                                │
│                      Angular Frontend (Port 4200)                            │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐        │
│  │  Dashboard  │  │   Server    │  │    Pod      │  │   Resource  │        │
│  │    View     │  │    List     │  │  Management │  │   Monitor   │        │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘        │
└─────────────────────────┬───────────────────────────────────────────────────┘
                          │ HTTP/REST API Calls
                          ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                          BACKEND API LAYER                                    │
│                      Flask Python Server (Port 5000)                         │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐        │
│  │GET /servers │  │POST /create │  │POST /update │  │POST /delete │        │
│  │             │  │             │  │             │  │             │        │
│  │List servers │  │Create new   │  │Update pod   │  │Delete pod   │        │
│  │and pods     │  │pod          │  │resources    │  │             │        │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘        │
└─────────────────────────┬───────────────────────────────────────────────────┘
                          │ File I/O Operations
                          ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                           DATA LAYER                                          │
│                      JSON Storage (mock_db.json)                             │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐        │
│  │   Servers   │  │    Pods     │  │  Resources  │  │   Status    │        │
│  │             │  │             │  │             │  │             │        │
│  │• ID & Name  │  │• Pod ID     │  │• GPUs       │  │• Running    │        │
│  │• Resources  │  │• Owner      │  │• RAM (GB)   │  │• Pending    │        │
│  │• Status     │  │• Image      │  │• Storage    │  │• Failed     │        │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘        │
└─────────────────────────┬───────────────────────────────────────────────────┘
                          │ Kubernetes API Integration
                          ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                      KUBERNETES CLUSTER                                       │
│                   Container Orchestration Platform                           │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐        │
│  │ Namespaces  │  │    Pods     │  │  Services   │  │   Ingress   │        │
│  │             │  │             │  │             │  │             │        │
│  │• test-team  │  │• Running    │  │• ClusterIP  │  │• Routes     │        │
│  │• prod-team  │  │• Pending    │  │• NodePort   │  │• SSL/TLS    │        │
│  │• dev-team   │  │• Failed     │  │• LoadBalancer│  │• Domains    │        │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘        │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Process Flow Diagrams

### 1. Resource Viewing Process
```
[User] → [Dashboard] → [GET /servers] → [Backend] → [Read JSON] → [Display Data]
   ↑                                                                      ↓
   └─────────────────── [Update UI] ← [Format Response] ← [Return JSON] ←─┘
```

### 2. Pod Creation Process
```
[User] → [Create Form] → [Validate Input] → [POST /create] → [Backend]
                                                                 ↓
[Deploy K8s] ← [Update JSON] ← [Check Resources] ← [Validate Request]
     ↓
[Success Response] → [Update UI] → [Refresh Dashboard]
```

### 3. Resource Management Process
```
[Resource Request] → [Availability Check] → [Allocation Logic] → [Update Database]
        ↓                    ↓                      ↓                    ↓
[User Interface] ← [Error Handling] ← [Resource Validation] ← [Kubernetes Sync]
```

## Key Features & Capabilities

### Resource Tracking
- **GPU Management**: Track high-performance computing resources
- **Memory Allocation**: RAM management in GB units
- **Storage Management**: Disk space allocation and monitoring
- **CPU Resources**: Processing unit allocation

### Multi-Tenancy
- **Team-based Namespaces**: Separate environments for different teams
- **Resource Quotas**: Prevent resource over-allocation
- **Access Control**: Team-based permissions and ownership

### Container Orchestration
- **Docker Integration**: Support for various container images
- **Kubernetes Native**: Direct K8s API integration
- **Lifecycle Management**: Pod creation, updates, and deletion
- **Status Monitoring**: Real-time pod and resource status

### API Design
- **RESTful Architecture**: Standard HTTP methods and status codes
- **JSON Payloads**: Structured data exchange
- **Error Handling**: Comprehensive error responses
- **Documentation**: Swagger/OpenAPI integration

This diagram can be imported into Microsoft Visio or similar tools for further customization.
