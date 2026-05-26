#  Unit 1: Getting Started with Docker

## Understanding Docker – What It Is and What It Does

Docker is a powerful software platform designed to help developers build, distribute, and run applications inside lightweight, isolated environments known as containers.  
A container bundles together an application and everything it needs to function—such as libraries, system tools, code, and runtime settings. This guarantees that the application will behave identically whether it's running on a developer's laptop, a test server, or a production cloud system.

---

## Why Docker? 

- Permanently solves the frustrating but "it works on my machine" problem by making environments consistent across all stages of development and deployment.
- Containers are exceptionally lightweight—they share the host operating system's kernel, avoiding the overhead of running multiple full operating systems.
-  Start and stop containers in seconds, dramatically speeding up development cycles, testing, and deployment processes.
- Scaling applications becomes straightforward: you can spin up many container instances of the same image to handle increased traffic or workload.

---

##  Comparing Docker Containers with Traditional Virtual Machines

| Feature     |  Docker (Containers) |  Virtual Machines (VMs) |
|------------|------------------------|----------------------------|
| Size       | Very small footprint (megabytes to a few hundred MB) | Large (gigabytes per VM, including a full OS) |
| Boot Time  | Starts almost instantly (often less than a second) | Can take minutes to boot the guest OS |
| Performance | Near-native speed because there's no hypervisor or full OS emulation | Slower due to hardware emulation and the overhead of running a separate OS |
| Isolation  | Process-level isolation using kernel namespaces and cgroups | Full hardware-level isolation, each VM is a complete operating system |

---

## Docker's Internal Architecture – Main Components

- **Docker Client** → The command-line tool (e.g., `docker` command) that users interact with. It sends instructions to the Docker Daemon.
- **Docker Daemon** → The background service running on the host machine that accepts requests from the client and actually creates, runs, and manages containers.
- **Docker Images** → Read-only templates or blueprints that define everything needed for a container: the application code, runtime, dependencies, and configuration.
- **Docker Containers** → Live, running instances of a Docker image. You can start, stop, move, or delete a container – each is an executable unit of software.

---

## Essential Docker Commands for Beginners

Below are some fundamental commands to verify your Docker installation and start exploring:

```bash
docker --version   # Prints the installed Docker version (e.g., 24.0.7)
docker info        # Shows detailed system-wide information: number of containers, images, storage driver, and more
docker help        # Lists all available Docker commands with brief descriptions; can be followed by a specific command for deeper help