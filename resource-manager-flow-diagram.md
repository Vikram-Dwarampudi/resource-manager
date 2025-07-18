# Resource Manager - System Flow Diagram

## Overview
This diagram shows the high-level architecture and workflow of the Resource Manager application.

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                                USER INTERFACE                                          │
│                         (Angular Frontend - Port 4200)                                 │
└─────────────────────────┬───────────────────────────────────────────────────────────┘
                          │
                          │ HTTP/REST API Calls
                          │
┌─────────────────────────▼───────────────────────────────────────────────────────────┐
│                              BACKEND API                                               │
│                         (Flask Python - Port 5000)                                     │
│                                                                                         │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  │
│  │   GET /servers  │  │   POST /create  │  │   POST /update  │  │   POST /delete  │  │
│  │                 │  │                 │  │                 │  │                 │  │
│  │ List all servers│  │   Create new    │  │   Update pod    │  │   Delete pod    │  │
│  │   and pods      │  │     pod         │  │   resources     │  │                 │  │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘  └─────────────────┘  │
└─────────────────────────┬───────────────────────────────────────────────────────────┘
                          │
                          │ Read/Write Operations
                          │
┌─────────────────────────▼───────────────────────────────────────────────────────────┐
│                            DATA LAYER                                                  │
│                         (mock_db.json)                                                 │
│                                                                                         │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐                       │
│  │    Servers      │  │      Pods       │  │    Resources    │                       │
│  │                 │  │                 │  │                 │                       │
│  │  • ID & Name    │  │  • Pod ID       │  │  • GPUs         │                       │
│  │  • Status       │  │  • Owner        │  │  • RAM (GB)     │                       │
│  │  • Timestamps   │  │  • Status       │  │  • Storage (GB) │                       │
│  │                 │  │  • Docker Image │  │  • CPUs         │                       │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘                       │
└─────────────────────────┬───────────────────────────────────────────────────────────┘
                          │
                          │ Kubernetes API Integration
                          │
┌─────────────────────────▼───────────────────────────────────────────────────────────┐
│                      KUBERNETES CLUSTER                                                │
│                    (Optional Integration)                                              │
│                                                                                         │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐                       │
│  │    Namespaces   │  │      Pods       │  │    Services     │                       │
│  │                 │  │                 │  │                 │                       │
│  │  • test-team    │  │  • Running      │  │  • Networking   │                       │
│  │  • prod-team    │  │  • Pending      │  │  • Load Balancer│                       │
│  │  • dev-team     │  │  • Failed       │  │                 │                       │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘                       │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## Key Workflows

### 1. View Resources
```
User → Frontend → GET /servers → Backend → Read mock_db.json → Display server list with resource usage
```

### 2. Create New Pod
```
User → Frontend → Fill form → POST /create → Backend → Validate resources → Update mock_db.json → Deploy to K8s
```

### 3. Update Pod Resources
```
User → Frontend → Edit dialog → POST /update → Backend → Check availability → Update allocations → Sync with K8s
```

### 4. Delete Pod
```
User → Frontend → Confirm deletion → POST /delete → Backend → Remove from mock_db.json → Delete from K8s
```

## Technology Stack

- **Frontend**: Angular 20.1.0 with Material Design
- **Backend**: Flask with CORS, Swagger documentation
- **Data**: JSON file-based storage (mock_db.json)
- **Container Orchestration**: Kubernetes integration
- **API**: RESTful endpoints with JSON payloads

## Key Features

- **Resource Management**: Track GPU, RAM, storage, and CPU allocation
- **Multi-tenant**: Support for different teams/owners
- **Real-time Updates**: Consistency checks and resource validation
- **Container Integration**: Docker image management
- **Kubernetes Native**: Direct K8s API integration for pod lifecycle management

## Server Resources Tracked

- **GPUs**: High-performance computing resources
- **RAM**: Memory allocation in GB
- **Storage**: Disk space in GB  
- **CPUs**: Processing units

Each server tracks both **total** and **available** resources to prevent over-allocation.