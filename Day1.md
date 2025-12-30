### Docker 
- image is a template
- container is running instance of a image 
- docker is a platform it let us package an app with additions needed
  
### Docker image 

- A blueprint / template
- Read-only
- Contains:
- OS layer (minimal Linux)
- App
- Dependencies

### Docker container 

- A running instance of an image
- Created from an image
- Has:
- Running process
- Memory
- Network
- File system (temporary)

| Real world              | Docker    |
| ----------------------- | --------- |
| Recipe                  | Image     |
| Dish cooked from recipe | Container |
| Blueprint               | Image     |
| House                   | Container |

Docker exists to solve environment consistency issues
VM = full OS per app
Docker = shared OS kernel

Docker architecture:
Client → Daemon → Image → Container

docker --version → check installation
docker info → check daemon
docker run hello-world → test Docker

┌─────────────────────────────┐
│        Docker Client        │  ← docker commands
│        (docker CLI)         │
└──────────────┬──────────────┘
               │ REST API
┌──────────────▼──────────────┐
│        Docker Daemon         │  ← brain
│           (dockerd)          │
│  - builds images             │
│  - runs containers           │
│  - manages networks/volumes  │
└──────────────┬──────────────┘
               │
┌──────────────▼──────────────┐
│     containerd + runc       │  ← execution
│  - low-level container ops  │
└──────────────┬──────────────┘
               │
┌──────────────▼──────────────┐
│        Linux Kernel         │
│   (namespaces, cgroups)     │
└─────────────────────────────┘
the above is the docker enginer 

`Commands`

- docker ps // process status // shows running container
- docker ps -a // shows all the container stopped, exited, running
- docker run ubuntu
- docker pull ubuntu
- docker stop <id>
- docker rm <id>
- docker run -it ubuntu
Docker Engine Flow — Summary
Overview

Docker Engine is a client–server architecture that uses the Linux kernel to create and run containers.
A container is not a virtual machine; it is a Linux process with isolation.

`High-Level Architecture
Docker Client (CLI)
        ↓  REST API
Docker Daemon (dockerd)
        ↓
containerd
        ↓
runc
        ↓
Linux Kernel (namespaces, cgroups)`

Component-wise Flow
### 1. Docker Client (CLI)

Command-line tool used by the user

Examples:

`docker run ubuntu
docker ps
docker images`


**Responsibilities:**

- Parses user commands
- Converts them into REST API requests
- Does not create or run containers

`2. Docker REST API`

Communication layer between:
- Docker Client
- Docker Daemon

`Uses:`

- nix socket (/var/run/docker.sock) on Linux / WSL2
- Named pipes on Windows

`3. Docker Daemon (dockerd)`

Core service running in the background

**Responsibilities:**

- Pull images from registries
- Create container metadata
- Configure networking and volumes
- Acts as the control plane of Docker

`4. containerd`

Low-level container management daemon

**Responsibilities:**

- Manages container lifecycle
- Handles image layers and snapshots
- Interfaces between dockerd and runtime
- Used by Kubernetes as well

`5. runc`

Low-level container runtime

**Responsibilities**:

- Creates Linux namespaces (PID, NET, MNT, UTS)
- Applies cgroups (CPU, memory limits)
- Starts the container’s main process
- After execution, the container becomes a normal Linux process

`6. Linux Kernel`

Final authority that runs the container process

**Provides:**

- Process scheduling
- Isolation (namespaces)
- Resource control (cgroups)
- Docker depends entirely on kernel features

End-to-End Command Flow
```
User types: docker run ubuntu
↓
Docker CLI sends REST API request
↓
dockerd processes the request
↓
containerd manages execution
↓
runc creates isolated process
↓
Linux kernel runs the process
```
**Key Rules (Important)**

- Container = Linux process
- No process running → container stops
- Docker does not virtualize hardware
- Docker does not create an OS
- Containers share the host OS kernel

| Feature        | Virtual Machine | Docker Container   |
| -------------- | --------------- | ------------------ |
| Kernel         | Separate per VM | Shared host kernel |
| OS             | Full Guest OS   | No Guest OS        |
| Startup        | Slow            | Fast               |
| Resource usage | High            | Low                |


> Docker Engine follows a client–server architecture where the Docker CLI communicates with the Docker daemon via REST APIs. The daemon delegates container lifecycle operations to containerd, which invokes runc to create isolated Linux processes using namespaces and cgroups provided by the Linux kernel.