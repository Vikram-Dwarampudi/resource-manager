# Resource Manager - Clean System Architecture

## System Overview

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           USER INTERFACE LAYER                                │
└─────────────────────────┬───────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                          BACKEND API LAYER                                    │
│                                                                               │
│  GET /servers   POST /create   POST /update   POST /delete   GET /consistency │
└─────────────────────────┬───────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                            DATA LAYER                                         │
│                                                                               │
│  Server Information   Pod Management   Resource Allocation   Consistency Data │
└─────────────────────────┬───────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                      KUBERNETES CLUSTER                                       │
│                                                                               │
│  Namespaces   Pod Status   Services   Resource Monitoring                    │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Component Layers

### User Interface Layer
- Dashboard
- Server List  
- Pod Management
- Resource Monitor

### Backend API Layer
- GET /servers
- POST /create
- POST /update
- POST /delete
- GET /consistency-check

### Data Layer
- Server Information
- Pod Management
- Resource Allocation
- Consistency Data

### Kubernetes Cluster
- Namespaces
- Pod Status
- Services
- Resource Monitoring

## System Workflows

1. **View Resources**: User → Dashboard → GET /servers → Backend → JSON Database → Display
2. **Create Pod**: User → Create Form → POST /create → Backend → Validate → Update JSON → Deploy K8s
3. **Update Pod**: User → Edit Dialog → POST /update → Backend → Update → Sync K8s → Refresh UI
4. **Delete Pod**: User → Confirm → POST /delete → Backend → Remove JSON → Delete K8s → Free Resources
5. **Consistency Check**: System → GET /consistency-check → Backend → Validate → Check Integrity → Report Status
