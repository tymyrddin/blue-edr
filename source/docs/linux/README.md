# Introduction

## What?

Endpoint Detection and Response (EDR) for Linux monitors system activity in real time, detecting and responding to 
threats like malware, unauthorized access, and suspicious behaviour. It leverages kernel-level visibility (eBPF, auditd) 
and open-source tools (Falco, Osquery) to secure servers, cloud workloads, and containers.

## Why?

* Threats are evolving: Attackers increasingly target Linux systems (cloud servers, IoT, DevOps pipelines).
* Limited native security: Unlike Windows/macOS, Linux lacks built-in EDR capabilities.
* Critical for compliance: Required for frameworks like CIS, NIST, and PCI-DSS.

## How?

* [Kernel-level monitoring](kernel.md)
* [Filesystem integrity](fs.md)
* [Container security](container.md)
* [Threat Hunting with Open Source](hunting.md)