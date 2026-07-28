---
layout: project
title: "Orange Sentry IoT Honeypot"
slug: iot-honeypot
status: "Hiatus - Pre-Alpha"
tags: [C, Python, MQTT, Embedded Linux, cybersecurity]
---

> **Status:** Pre-Alpha | Milestone 0

Welcome to the home of the **Orange Sentry** project!

Orange Sentry is a portable, hardware-controlled, embedded honeypot and IDS system based on the Orange Pi Zero 3 (2GB) Single Board Computer.

It uses battle-tested open-source tools like **Cowrie** and **Suricata** for a robust detection engine, orchestrated by a custom **Control Plane** written in C. The goal is to package high-level network intrusion detection into a cheap, low-power, and tactile device for security students, researchers, practitioners, and overall security nerds like me.

## The Objective: Bridging the Gap
This project exists to bridge the gap between **High-Level Threat Analysis** and **Low-Level Hardware Constraints**.

While this is primarily a "learn-by-doing" project to help me become a better "full-stack" IoT Engineer, learning Hardware, Firmware, Application, Connectivity, and Security domains. My plan is to ship a genuinely cool, functional product by the end.

## Architecture & Technical Minutia
The Orange Sentry works as a physical system with two distinct planes of operation: the *Enforcement Plane* and the *Control Plane*. 

### 1. The Enforcement Plane
This layer handles the heavy lifting of network traffic, security, connectivity, and detection.
* **OS:** DietPi (Armbian based) optimized for minimal footprint.
* **The Detection:** Runs **Cowrie**, a medium-interaction SSH/Telnet Honeypot, and **Suricata**, a Network-based Intrusion Detection System (NIDS).
* **Hardware Interaction:** Tools and scripts for controlling the physical board, such as thermal management (fan control) written in C and the display server written in Python.
* **Connectivity:** A lightweight MQTT client based on **Eclipse Paho** that sends telemetry and receives commands from an authenticated broker.

### 2. The Control Plane
This is the custom embedded logic that acts as the "brain" of the board. These tools orchestrate the Enforcement Layer based on the device's state.
* **FSM (Finite State Machine):** Written in C, it manages the logic of the device and toggles functionalities based on the current mode.
* **MQTT Controller:** A service to handle identity, authenticate the broker, and execute commands received via the network.

### 3. The Hardware
* **Core:** Orange Pi Zero 3 (2GB RAM) running DietPi.
* **Storage:** 64 GB SanDisk SD card.
* **Interface:** Integrated case with 3 push buttons (Up, Down, Select) and an OLED I2C display.
* **Thermal Management:** A custom fan and heatsink assembly. I am writing a small daemon to monitor thermal zones and trigger the fan only when approaching the upper bound of the optimal range.

![Full Architecture Diagram](/assets/images/Orange-Pi-Architecture.drawio.png)

## Operational Modes
The device logic is based on a Finite State Machine controlled via physical buttons or authenticated MQTT commands.

| Mode | Description |
| :--- | :--- |
| **🔒 Closed Mode** | All network ports are closed via Nftables. Interaction is limited to the physical user interface. |
| **👂 Passive Listen** | Opens ports but silently drops connection attempts, logging the traffic meta-data. |
| **🍯 Honeypot** | Engages the Cowrie (SSH/Telnet) honeypot and Suricata NIDS to generate alerts and capture attacker sessions. |
| **🛠️ Development** | Disengages the honeypot/IDS and allows direct legitimate SSH connections for maintenance. |


![Orange Pi Working Modes](/assets/images/Orange-Pi-OrangePIWorkingModes.drawio.png)

## Engineering Challenges
I chose not to reinvent the wheel with the detection engines (Cowrie/Suricata), but I am building the infrastructure from scratch to solve specific engineering hurdles:

1.  **The Cross-Compilation Pivot:** Migrating from compiling on-device (which is slow and wears out the SD card) to a robust cross-compilation toolchain on my x86 host.
2.  **Deterministic Memory:** Moving away from standard `malloc/free` to **Arena Allocators** to ensure the device can run 24/7 without memory leaks.
3.  **Secure C2:** Implementing MQTT over TLS with mutual authentication on a resource-constrained device.

## Development Philosophy
I am leveraging an age-old engineering adage that I picked up from professional "non-engineer engineering extraordinaire" Martin from the [Wintergatan YouTube Channel](https://www.youtube.com/@Wintergatan):

> "If you can buy the part off-the-shelf, you don't make the part."

However, I am adding a personal spin:
> "If you can buy the part off-the-shelf, you don't make the part—**unless you want to learn how to make the part.**"

My focus is on optimizing existing tools to run comfortably on the tight 2GB RAM constraint while maintaining compatibility with upstream sources. But, I also reserve the right to build my own tooling whenever I want to deeply understand how something works under the hood.

## Roadmap
I am following a "Vertical Slice" development strategy, ensuring functional deliverables at every stage. Every sprint is supposed to represent one to two weeks of work, but as I am indeed learning about this as I go it is subject to change.

### Milestone 0: Pre-Alpha (Infrastructure and Minimal Viable Product)

- [x] Sprint 0: Initial setup (OS, Cowrie, Firewall rules, initial control pane mocking for validation, ETC.)
- [x] Sprint 1: Build System, Cross-Compilation & Toolchain
	- Goal: Create a solid build system with GNU make that is flexible and able to handle:
    1. Vendor libraries such as Paho MQTT C Library, and
    2. Internal util libraries.
	- Tasks:
		- Refactor Makefile stack to acocunt for cross compilation and linking
- [ ] Sprint 2: MQTT Connectivity and initial implementation
	- Goal: Implement a functional pub MQTT client using the Paho C Library and the Mosquitto Broker
	- Tasks:
		- Implement PUB.
		- Implement sending Heartbeats to the broker 
		- Recieve simple commands by subbing to command topics (ex: "change_mode on topic /board/control"). // added to next sprint
- Sprint 2.5: MQTT subscribing and security. Also cleanup
	- Goal: Functional pub/sub client reasonably bug-secure using callbacks and input sanitization
		- Tasks:
			- Implement sub with callbacks
			- Debug and validate MQTT functionality on ARM
			- Mock receiving commands via the MQTT sub input
			- Implement account segregation such that the account that runs the MQTT service isn't root
			- Implement input sanitization on the client both to parse messages to send and commands to receive (important security stuff)
- [ ] Sprint 3: Initial hardware interaction (buttons)
  - Goal: Implement button-press detection with adequate debouncing in a kernel module. Implement some sort of manager service for the hardware implementation.
  - Tasks:
    - Make a button press change the state of the FSM
    - Make the change in the FSM state trigger a log on MQTT
    - Small refactor on the main controller such that it can orchestrate cowrie's functionality according to the FSM state
- [ ] Sprint 4: OLED screen integration
  - Goal: integrate an OLED screen into the program that shows the current state and some actions you can take based on the state. You can select those actions via the buttons and they will meaningfully act on the board's functionality (e.g. changing states or closing current connections within suricata)
  - Tasks:
    - Write a C program that generates the information that is going to be rendered to the screen based on the context and sends it over via a jason on FIFO pipes 
    - Write a small rendering server with python that can read the JSON information, render it appropriately to a framebuffer and send it over to a simple I2C controlled OLED screen
- [ ] Sprint 5: Board security hardening and final controller implementation
  - Goal: make the board as secure as reasonably possible with both Linux security best practices and C Programming secuirity best practices 
    - Tasks:
      - Implement strong MAC via something like AppArmor
      - Make the production version of the board ROFS
      - Implement authentication on the MQTT client
      - Final refactoring of the controller
- [ ] Sprint 6: Final pre-alpha review
  - Goal: Make the project ready for the first alpha deliverables
  - Tasks:
    - Thorough testing of the board, including manual logic validation and automated benchmarking
    - Consolidating documentation and build instructions 
    - Consolidating README, milestone DevLog and first LinkedIn post about the project
    - Consolidating presentation strategy
    
### Milestone 1: Alpha (Device Reliability)
*Focus: Local Persistence, Hardware Watchdog, Thermal Management, and Crash Recovery.*

### Milestone 2: Beta (Ecosystem)
*Focus: Webapp Backend, Fleet Management (IaC), and Auto-Enrollment.*

---
```
<<<<<<< HEAD
*Built with ☕ and segfaults by [JoaoNasMont](https://github.com/JoaoNasMont).*
=======
*Built with ☕, segfaults and merge conflicts by [JoaoNasMont](https://github.com/JoaoNasMont).*
>>>>>>> 7658a3e (add images to project posts)
```
