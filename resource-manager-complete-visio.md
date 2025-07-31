# Resource Manager - Complete Visio-Style Flow Diagram

## System Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                           USER INTERFACE LAYER                                                  │
│                                    Angular Frontend (Port 4200)                                                │
│                                          Material Design                                                        │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐        │
│  │  Dashboard  │  │   Server    │  │    Pod      │  │   Resource  │  │    Edit     │  │ Consistency │        │
│  │    View     │  │    List     │  │  Management │  │   Monitor   │  │   Dialog    │  │    Check    │        │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘        │
└─────────────────────────┬───────────────────────────────────────────────────────────────────────────────────┘
                          │ HTTP/REST API Calls
                          ▼
┌─────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                         BACKEND API LAYER                                                       │
│                                    Flask Python Server (Port 5000)                                             │
│                                   CORS Enabled • Swagger Documentation                                         │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────────────────┐  │
│  │GET /servers │  │POST /create │  │POST /update │  │POST /delete │  │    GET /consistency-check           │  │
│  │             │  │             │  │             │  │             │  │                                     │  │
│  │List all     │  │Create new   │  │Update pod   │  │Delete pod   │  │Validate system consistency          │  │
│  │servers and  │  │pod with     │  │resources    │  │and free     │  │and resource integrity               │  │
│  │pods with    │  │resource     │  │and config   │  │allocated    │  │Check JSON vs K8s alignment         │  │
│  │status       │  │validation   │  │             │  │resources    │  │                                     │  │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────────────────────────────┘  │
└─────────────────────────┬───────────────────────────────────────────────────────────────────────────────────┘
                          │ Read/Write Operations • JSON File I/O
                          ▼
┌─────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                           DATA LAYER                                                            │
│                                    JSON Storage (mock_db.json)                                                 │
│                                        Persistent Data Storage                                                  │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐      │
│  │     Server      │  │       Pod       │  │    Resource     │  │     Status      │  │   Consistency   │      │
│  │   Information   │  │   Management    │  │   Allocation    │  │   Tracking      │  │      Data       │      │
│  │                 │  │                 │  │                 │  │                 │  │                 │      │
│  │• ID & Name      │  │• Pod ID         │  │• GPUs           │  │• Running        │  │• Resource       │      │
│  │• Resources      │  │• Owner          │  │• RAM (GB)       │  │• Pending        │  │  Tracking       │      │
│  │• Status         │  │• Status         │  │• Storage (GB)   │  │• Failed         │  │• Validation     │      │
│  │• Timestamps     │  │• Docker Images  │  │• CPU            │  │• Terminating    │  │  Status         │      │
│  │• Availability   │  │• Namespaces     │  │• Total/Available│  │• Created        │  │• Integrity      │      │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘  └─────────────────┘  └─────────────────┘      │
└─────────────────────────┬───────────────────────────────────────────────────────────────────────────────────┘
                          │ Kubernetes API Integration • Container Orchestration
                          ▼
┌─────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                      KUBERNETES CLUSTER                                                         │
│                                Container Orchestration Platform                                                 │
│                                    Pod Lifecycle Management                                                     │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐      │
│  │   Namespaces    │  │   Pod Status    │  │    Services     │  │   Resource      │  │   Monitoring    │      │
│  │                 │  │                 │  │                 │  │   Monitoring    │  │   & Health      │      │
│  │• test-team      │  │• Running        │  │• ClusterIP      │  │• Usage Metrics  │  │• Health Checks  │      │
│  │• prod-team      │  │• Pending        │  │• NodePort       │  │• Allocation     │  │• Performance    │      │
│  │• dev-team       │  │• Failed         │  │• LoadBalancer   │  │• Limits         │  │• Alerts         │      │
│  │• staging        │  │• Terminating    │  │• Ingress        │  │• Requests       │  │• Logs           │      │
│  │• default        │  │• Succeeded      │  │• NetworkPolicy  │  │• Quotas         │  │• Events         │      │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘  └─────────────────┘  └─────────────────┘      │
└─────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

## Complete System Workflows

### 1. View Resources Flow
```
┌─────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  User   │───▶│  Dashboard  │───▶│GET /servers │───▶│   Backend   │───▶│ Read JSON   │───▶│Display Data │
│ Action  │    │    View     │    │ API Call    │    │ Processing  │    │ Database    │    │   to UI     │
└─────────┘    └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
```

### 2. Create Pod Flow
```
┌─────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  User   │───▶│Create Form  │───▶│POST /create │───▶│   Backend   │───▶│Check        │───▶│Update JSON  │
│ Input   │    │Validation   │    │ API Call    │    │ Validation  │    │Resources    │    │Database     │
└─────────┘    └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
                                                                                                     │
┌─────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐              │
│ Success │◀───│  Update UI  │◀───│   Return    │◀───│  Deploy to  │◀───│Kubernetes   │◀─────────────┘
│Response │    │  Dashboard  │    │  Response   │    │Kubernetes   │    │Integration  │
└─────────┘    └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
```

### 3. Update Pod Flow
```
┌─────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  User   │───▶│Edit Dialog  │───▶│POST /update │───▶│   Backend   │───▶│Validate     │───▶│Update       │
│ Edit    │    │Modifications│    │ API Call    │    │ Processing  │    │Changes      │    │Allocations  │
└─────────┘    └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
                                                                                                     │
┌─────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐              │
│Refresh  │◀───│  Update UI  │◀───│   Sync      │◀───│Update JSON  │◀───│Sync with    │◀─────────────┘
│Dashboard│    │  Display    │    │ Complete    │    │Database     │    │Kubernetes   │
└─────────┘    └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
```

### 4. Delete Pod Flow
```
┌─────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  User   │───▶│Confirm      │───▶│POST /delete │───▶│   Backend   │───▶│Remove from  │───▶│Delete from  │
│ Delete  │    │Deletion     │    │ API Call    │    │ Processing  │    │JSON         │    │Kubernetes   │
└─────────┘    └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
                                                                                                     │
┌─────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐              │
│Update   │◀───│  Refresh    │◀───│   Free      │◀───│Update       │◀───│Cleanup      │◀─────────────┘
│UI       │    │  Dashboard  │    │ Resources   │    │Availability │    │Resources    │
└─────────┘    └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
```

### 5. Consistency Check Flow
```
┌─────────┐    ┌─────────────┐    ┌─────────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│System/  │───▶│Trigger      │───▶│GET /consistency-│───▶│   Backend   │───▶│Validate     │───▶│Check        │
│User     │    │Check        │    │check API Call  │    │ Processing  │    │JSON vs K8s  │    │Integrity    │
└─────────┘    └─────────────┘    └─────────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
                                                                                                     │
┌─────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐              │
│Report   │◀───│  Auto-fix   │◀───│   Return    │◀───│Generate     │◀───│Analyze      │◀─────────────┘
│Status   │    │  Issues     │    │  Report     │    │Report       │    │Discrepancies│
└─────────┘    └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
```

## Detailed Component Interactions

### Frontend Components
- **Dashboard**: Main overview with server status and resource utilization
- **Server List**: Detailed view of all servers with real-time status
- **Pod Management**: Create, edit, delete pod operations
- **Resource Monitor**: Real-time resource usage and availability
- **Edit Dialog**: Modal for updating pod configurations
- **Consistency Check**: System validation and integrity reports

### Backend API Endpoints
- **GET /servers**: Returns complete server list with pod information and resource allocation
- **POST /create**: Validates resources, creates pod entry, deploys to Kubernetes
- **POST /update**: Updates pod configuration, validates resources, syncs with Kubernetes
- **POST /delete**: Removes pod, frees resources, cleans up Kubernetes deployment
- **GET /consistency-check**: Validates data integrity between JSON storage and Kubernetes

### Data Management
- **Server Information**: Server ID, name, total/available resources, status, timestamps
- **Pod Management**: Pod ID, owner, status, Docker images, namespace assignments
- **Resource Allocation**: GPU, RAM, storage, CPU tracking with total/available calculations
- **Status Tracking**: Real-time status updates for running, pending, failed, terminating states
- **Consistency Data**: Resource tracking validation and integrity status

### Kubernetes Integration
- **Namespaces**: Multi-tenant isolation (test-team, prod-team, dev-team, staging, default)
- **Pod Status**: Complete lifecycle management (running, pending, failed, terminating, succeeded)
- **Services**: Network services (ClusterIP, NodePort, LoadBalancer, Ingress, NetworkPolicy)
- **Resource Monitoring**: Usage metrics, allocation tracking, limits, requests, quotas
- **Monitoring & Health**: Health checks, performance monitoring, alerts, logs, events

## Key Features & Capabilities

### Resource Management
- **GPU Allocation**: High-performance computing resource tracking and allocation
- **Memory Management**: RAM allocation and monitoring in GB units
- **Storage Management**: Disk space allocation and usage tracking
- **CPU Resources**: Processing unit allocation and utilization monitoring
- **Resource Validation**: Prevent over-allocation and ensure availability

### Multi-Tenancy Support
- **Team-based Namespaces**: Isolated environments for different teams
- **Resource Quotas**: Prevent resource over-allocation per team
- **Access Control**: Team-based permissions and ownership
- **Namespace Management**: Automatic namespace creation and management

### Container Orchestration
- **Docker Integration**: Support for various container images and registries
- **Kubernetes Native**: Direct K8s API integration for all operations
- **Lifecycle Management**: Complete pod lifecycle from creation to termination
- **Status Monitoring**: Real-time pod and resource status tracking
- **Health Checks**: Automatic health monitoring and failure detection

### System Integrity
- **Consistency Validation**: Regular checks between JSON storage and Kubernetes
- **Resource Integrity**: Validation of resource allocation accuracy
- **Auto-healing**: Automatic correction of detected inconsistencies
- **Audit Trail**: Complete tracking of all system operations
- **Error Handling**: Comprehensive error detection and recovery

### API Design
- **RESTful Architecture**: Standard HTTP methods and status codes
- **JSON Payloads**: Structured data exchange format
- **Error Handling**: Comprehensive error responses with detailed messages
- **Documentation**: Swagger/OpenAPI integration for API documentation
- **CORS Support**: Cross-origin resource sharing for web applications

This comprehensive diagram can be imported into Microsoft Visio, Lucidchart, or similar tools for further customization and presentation.