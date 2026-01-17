---
layout: project_page
title: "IoT Honeypot"
description: "An IoT Honeypot device built around an Orange Pi Zero 3 2GB leveraging open source honeypots like Cowrie to catch threat actors on networks."
date: 2026-01-15
status: "Ongoing" 
tech_stack: [C, Python, MQTT, orange PI]
repo_url: "to-be-added"
slug: iot-honeypot   
---

# Orange Sentry IoT Honeypot
> Status: Pre-Alpha

Welcome to the home of the *Orange SentryI IoT Honeypot project!

Orange Sentry is an embedded honeypot and IDS system based around the Orange Pi Zero 3 2GB. It leverages OSS tools such as Cowrie and Suricata for a robust detection engine and some of my own code to package it all together into a cheap but powerful detection device for people interested insecurity such as researchers, students, hobbyists and practitioners.

My objectives with this project are to learn "full-stack" IoT engineering (meaning hardware, firmware, applications, connectivity and security) by doing. This is certainly an ambitious project, but I hope the sheer vastness of the technology that I will need to learn to use and configure to get even an MVP of this project up and running will keep me motivated.

## Current status
The project is currently in the **Pre-Alpha** state.

The board has been acquired and configured.
The Cowrie service has been installed and configured to listen to SSH and TELNET traffic, as well as to be managed by a systemd service.

My next steps will be to implement the core controller's business logic.

## Tech Stack (Abridged)
The product is built around an Orange Pi Zero 3 2Gb board, and is designed primarily around a small OLED display and some buttons on the case. This will allow the user to switch between different modes, which will give different functionality to the board: closed, passive listen, honeypot, and development.

Overall this project will help me learn about and get hands on practice with some very cool technologies, such as:
C, Python, MQTT, Embedded Linux, Hardware Drivers, I2C, Software Architecture, and others.

I also have some other stuff planned, such as a GOLANG webapp to allow remote control and observability leveraging MQTT and a correlation engine like Wazuh to correlate logs from many Orange Sentry devices at the same time, but meanwhile I am focusing on developing the best board I can while I learn along the way.

## Technical Minutia

### The Hardware
* **Core:** Orange Pi Zero 3 (2GB RAM) running DietPi.
* **Storage:** 64 GB SanDisk SD card.
* **Interface:** Integrated case with 3 push buttons (Up, Down, Select) and an OLED I2C display.
* **Thermal Management:** A custom-managed fan and heatsink assembly. I plan to write a small daemon to monitor thermal zones and trigger the fan only when approaching the upper bound of the optimal range.

### The Software Stack
I am prioritizing learning lower-level concepts. While I am not reinventing the wheel for complex detection engines (using off-the-shelf Cowrie and Suricata), I am writing my own tooling for the embedded logic.

* **Languages:** Python (for prototyping/display server) and C (for drivers/performance).
* **Protocols:** MQTT for remote control and observability.
* **Planned Tooling:** Custom drivers for button management, a port-control service, and secure IPC (Inter-Process Communication).

### Operational Modes
The device logic is based on a Finite State Machine controlled via the physical buttons or MQTT commands.

| Mode | Description |
| :--- | :--- |
| **Closed Mode** | All network ports are closed. Interaction is limited to the physical interface. |
| **Passive Listen** | Opens ports but drops connection attempts, logging the traffic. Authorized control is allowed via an authenticated MQTT broker. |
| **Honeypot** | Engages the Cowrie (SSH/Telnet) honeypot and Suricata NIDS to generate alerts. |
| **Development** | Disengages the honeypot/IDS and allows direct legitimate SSH connections for maintenance. |

## Design Philosophy
I am leveraging an age-old engineering adage that I picked up from professional non-engineer engineer Martin from the [Wintergatan YouTube Channel](https://www.youtube.com/@Wintergatan):

> "If you can buy the part off-the-shelf, you don't make the part."

However, I am adding a personal spin:
> "If you can buy the part off-the-shelf, you don't make the part—**unless you want to learn how to make the part.**"

I decided not to reinvent the wheel with the honeypots (I want to eventually ship this thing), so I am utilizing technologies built by people much smarter than me. My focus is on optimizing these tools to run comfortably on the tight 2GB RAM constraint while maintaining compatibility with upstream sources.

## Future Roadmap
* **Webapp:** A Go (Golang) web application for remote control and observability.
* **Fleet Management:** Integration with a correlation engine like **Wazuh** to aggregate logs from multiple Orange Sentry devices.
* **Optimization:** Migrating Python prototypes to C as the project matures.
