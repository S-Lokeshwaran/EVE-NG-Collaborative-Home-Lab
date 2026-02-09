# EVE-NG Collaborative Home Lab

## Overview
This project documents the setup of a personal EVE-NG based networking lab,
hosted on an old Linux laptop and accessed remotely using secure peer-to-peer connectivity.

## Objectives
- Practice enterprise-style network topologies
- Enable remote access from multiple devices
- Understand performance limits of real hardware
- Learn troubleshooting beyond simulations

## Tools Used
- Linux (Host OS)
- EVE-NG Community Edition
- QEMU/KVM
- Tailscale (WireGuard-based VPN)
- SSH & WinSCP

## Topology
![HOME-TOPOLOGY jpg](https://github.com/user-attachments/assets/b9de0cd2-fc19-4388-8e91-30b866f3cd86)


## Background & Motivation

As a networking learner, hands-on practice with realistic topologies is
essential to understand how enterprise networks behave beyond theory.

While simulations are useful, they often hide important aspects such as:
- Device boot time
- Resource constraints
- Real CLI responsiveness
- Troubleshooting under load

The motivation behind this project was to build a lab environment that:
- Runs continuously on personal hardware
- Can be accessed remotely from different machines
- Supports structured topology design
- Encourages learning through real limitations

This lab was not built to replace any existing infrastructure, but to
serve as a **personal learning and experimentation environment**, where
design decisions and constraints are clearly visible.

## Hardware Used & Constraints

The lab was hosted on a personal laptop that is significantly older than
typical server-grade hardware. The system specifications are listed below:

- Processor: Intel Core i5 (3rd Generation)
- Memory: 12 GB RAM (DDR3)
- Storage: 500 GB SSD
- Host Operating System: Linux

This hardware was intentionally reused to understand how modern
virtualized networking workloads behave under constrained resources.

### Why Hardware Constraints Matter

Network emulation platforms like EVE-NG rely heavily on CPU performance,
especially when running multiple router and switch images using QEMU.
Unlike lightweight simulations, these devices emulate real network
operating systems, which introduces measurable load on the host system.

Operating under these constraints provided valuable insights into:
- CPU saturation during device boot
- Memory usage patterns across multiple nodes
- The difference in behavior between lightweight end hosts and
  full-featured network devices
- The practical limits of scaling complex topologies on non-server hardware

These constraints directly influenced topology size, node selection, and
the overall lab design decisions documented in this project.

## Software & Tools Used

This lab was built using a combination of open-source tools commonly used
in real-world networking and systems environments. Each tool was selected
for a specific purpose, as explained below.

### Linux (Host Operating System)

A Linux-based operating system was used as the host due to its stability,
low resource overhead, and strong support for virtualization and
networking tools. Linux also provides native access to services such as
SSH, process monitoring, and package management, which are essential for
operating a long-running lab environment.

### EVE-NG Community Edition

EVE-NG Community Edition was chosen as the network emulation platform to
design and run enterprise-style network topologies. It allows multiple
virtual routers, switches, and end devices to be interconnected in a
single environment, closely resembling real network behavior.

EVE-NG was preferred over basic simulators because it exposes real
operating system behavior, including boot time, resource consumption,
and CLI interaction.

### QEMU / KVM

QEMU, combined with KVM acceleration, was used as the virtualization
backend for running network device images. This enables hardware-assisted
virtualization, which is critical for performance when emulating
enterprise network operating systems.

Despite hardware acceleration, QEMU-based network devices remain CPU
intensive, which influenced the scale of the topology used in this lab.

### Tailscale (WireGuard-based VPN)

Tailscale was used to provide secure remote access to the lab environment.
It enables encrypted, peer-to-peer connectivity between devices without
manual key exchange or complex VPN configuration.

This allowed the lab to be accessed from multiple laptops as if they were
on the same local network, simplifying remote CLI access and browser-based
console usage.

### SSH and WinSCP

SSH was used for secure command-line access to the Linux host and EVE-NG
backend, enabling system management, troubleshooting, and monitoring.

WinSCP was used to transfer device images and configuration files to the
server securely, reducing manual file handling and simplifying setup.


## Network Architecture & Topology Design

The lab topology was designed using a **three-tier enterprise network
architecture**, consisting of Core, Distribution, and Access layers.
This model was chosen to reflect commonly used designs in campus and
enterprise networks.


<img width="1650" height="842" alt="Screenshot 2026-02-08 174324" src="https://github.com/user-attachments/assets/52e284e2-e4f0-4bff-bcff-d0a429e8cf05" />


### Core Layer

The Core layer is responsible for high-speed packet forwarding and
interconnecting different parts of the network. In this lab, the Core
layer was implemented using routing-capable devices to simulate backbone
connectivity and inter-VLAN traffic flow.

The focus at this layer was not on edge configuration, but on:
- Fast routing decisions
- Redundant paths
- Stability under load

### Distribution Layer

The Distribution layer acts as a boundary between the Core and Access
layers. Layer 3 switches were placed in this layer to handle both
switching and routing functions.

L3 switches were used to:
- Perform inter-VLAN routing
- Act as default gateways for access networks
- Support first-hop redundancy concepts
- Enforce logical separation between access segments

This layer was designed to be the primary point for policy enforcement
and routing control.

### Access Layer

The Access layer connects end devices to the network. This layer was
implemented using Layer 2 switches and lightweight end hosts.

The Access layer focuses on:
- VLAN segmentation
- Port-level connectivity
- Simulating end-user access patterns

Lightweight end devices were intentionally used to reduce resource
consumption while still allowing realistic traffic generation and
testing.

### Redundancy and Link Design

Redundant links were introduced between layers to reflect high-availability
design principles commonly used in enterprise networks. These links were
included to explore how redundancy impacts topology behavior and to
support future testing of failover scenarios.

While full redundancy protocols were not always active due to hardware
constraints, the physical topology was designed to support them.

### Planned Protocols and Scenarios

The topology was structured to allow experimentation with protocols and
concepts such as:
- VLAN-based segmentation
- Inter-VLAN routing
- First-hop redundancy mechanisms
- Link failure and recovery behavior
- Traffic flow observation across layers

The emphasis of this design was on **architectural understanding** rather
than protocol-specific optimization.


## Lab Access & Collaboration Workflow

The lab was designed to be accessed and managed remotely, allowing
multiple client machines to interact with the same running topology.
This section describes how access to the lab was structured and how
collaboration was tested.

### Server Access

The EVE-NG lab runs on a Linux-based host system. Secure access to the
server was provided using SSH, which allowed:

- System monitoring and troubleshooting
- Managing EVE-NG services
- Transferring images and files
- Observing resource utilization during lab operation

SSH access enabled the lab to be managed without requiring direct
physical interaction with the server once it was configured.

### Browser-Based Device Access

EVE-NG’s HTML5 console feature was used to access routers, switches, and
end devices directly through a web browser.

This approach provided:
- No dependency on external terminal software
- Easy access from different laptops
- Isolation between device consoles
- A user-friendly way to observe device boot and runtime behavior

Each network device could be accessed independently through its console,
making it suitable for parallel configuration and testing.

### Remote Connectivity

Secure remote connectivity to the lab environment was established using
Tailscale. By placing the server and client laptops on the same private
virtual network, the lab could be accessed as if all devices were
connected locally.

This simplified:
- Remote SSH access
- Browser-based console usage
- Testing from different physical locations

No complex routing or manual tunnel configuration was required.

### Collaboration Model

The lab was tested for basic collaborative usage by accessing different
devices from separate client machines at the same time.

Typical usage included:
- One user observing or configuring core or distribution devices
- Another user interacting with access-layer devices or end hosts
- Simultaneous monitoring of topology behavior

While the Community Edition of EVE-NG has limitations regarding
multi-user topology management, console-level collaboration was
successfully demonstrated within these constraints.

## Performance Observations & System Behavior

Running enterprise-grade network device images on limited hardware
provided clear visibility into system behavior under load. This section
summarizes the key performance observations made during lab operation.

### Device Boot Behavior

A noticeable difference was observed between lightweight end devices and
full-featured network devices:

- Lightweight end hosts (such as VPCs) booted almost instantly
- Routers and Layer 3 switches required significantly more time to boot
- Multiple devices booting simultaneously increased overall startup time

This behavior is expected, as router and switch images emulate full
network operating systems, which are CPU intensive during initialization.

### CPU Utilization

During topology startup and active usage, CPU utilization on the host
system increased sharply. Monitoring tools such as `top` showed:

- High CPU usage during device boot phases
- Sustained load when multiple network devices were running
- Limited idle CPU time under full topology load

These observations highlight how emulated network devices place
continuous demand on processing resources, especially on older hardware.

### Memory Usage

Memory consumption remained relatively stable compared to CPU usage.
While multiple devices were active, available memory was sufficient due
to careful node selection and avoidance of unnecessary services.

This confirmed that CPU performance, rather than memory capacity, was
the primary limiting factor for this lab environment.

### Impact on Usability

Under higher load conditions, the following effects were observed:
- Slower device boot times
- Reduced responsiveness in some device consoles
- Increased delay when starting or stopping multiple nodes together

Despite these limitations, the lab remained usable for small to
medium-sized topologies, allowing meaningful configuration and testing.


## Limitations & Trade-offs

While the lab successfully demonstrated collaborative access and
enterprise-style topology design, several limitations were identified
during operation.

### Hardware Limitations

The primary constraint was CPU performance. Running multiple QEMU-based
router and switch images on older hardware resulted in high CPU
utilization, which limited the maximum number of devices that could be
run simultaneously with acceptable responsiveness.

As a result:
- Large-scale topologies were not practical on this system
- Devices were started in stages to reduce boot-time load
- Smaller, focused labs were preferred over full enterprise-scale builds

### Virtualization Overhead

The lab relied on nested virtualization, which introduces additional
overhead compared to running EVE-NG on bare-metal server hardware. This
impacted overall performance, particularly during simultaneous device
boot and intensive configuration tasks.

### Community Edition Constraints

EVE-NG Community Edition has inherent limitations related to multi-user
topology management. While console-level collaboration was achievable,
full concurrent design control is restricted.

These constraints required careful coordination during collaborative
testing and influenced how tasks were divided between users.

### Design Trade-offs

To maintain stability, trade-offs were made between realism and
scalability. Device selection, topology size, and protocol testing scope
were adjusted to ensure the lab remained usable and responsive.

These trade-offs were intentional and aligned with the project’s primary
goal of learning through realistic constraints.


## Key Learnings

This project provided several practical insights that go beyond
theoretical networking concepts.

- Realistic network emulation introduces measurable system constraints
  that directly influence design and testing decisions.
- CPU performance plays a critical role when emulating enterprise
  network devices, often more than memory capacity.
- Lightweight end hosts behave very differently from full-featured
  routers and switches in virtualized environments.
- Network architecture and topology planning must align with available
  infrastructure to maintain stability and usability.
- Secure remote access can be simplified significantly using modern
  peer-to-peer connectivity solutions.
- Collaboration is possible even in constrained environments when tasks
  are clearly scoped and access is well structured.

Overall, the project reinforced the importance of understanding not only
network protocols, but also the systems that support them.


## Conclusion & Future Improvements

This project successfully demonstrated that a personal, collaborative
networking lab can be built using limited hardware, provided that design
choices are made thoughtfully and constraints are clearly understood.

While the lab could not support large-scale enterprise topologies, it
proved effective for focused experiments involving routing, switching,
and architectural concepts. The experience emphasized learning through
real-world limitations rather than idealized simulations.

Future improvements to this lab may include:
- Migrating the setup to server-grade hardware for increased scalability
- Running EVE-NG directly on bare-metal to reduce virtualization overhead
- Expanding protocol testing under more stable conditions
- Improving collaboration workflows using role-based access models

This project serves as a foundation for continued learning and
experimentation in network engineering.


