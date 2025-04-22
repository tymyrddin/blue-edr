# Introduction

## What?

A security layer for macOS that detects and prevents malware, unauthorized executions, and data exfiltration. It uses 
Apple’s Endpoint Security Framework (ESF), System Integrity Protection (SIP), and XProtect to monitor processes, 
file changes, and network activity.

## Why?

* Macs are high-value targets: Attacks rose 300%+ in 2023 (Malwarebytes).
* Apple’s defenses aren’t enough: SIP/XProtect miss fileless attacks, adware, and zero-days.
* Organisational risk: Macs in organisational networks need zero-trust controls.

## How?

* [Process monitoring](process.md)
* [Memory protection](memory.md)
* [Behavioural detection](behavioural.md)
* [Network hardening](network.md)