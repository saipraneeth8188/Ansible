# Ansible Introduction

## What is Ansible?

Ansible is an **open-source automation tool** used for **configuration management**, **application deployment**, and **task automation**. It is **agentless**, which means it doesn't require any special software to be installed on the managed nodes. It communicates over SSH for Linux and WinRM for Windows.

## Why Ansible?

- Simple to learn (uses YAML syntax)
- No agent installation required
- Works over SSH (secure communication)
- Scalable for large infrastructures
- Idempotent: Re-running the same playbook doesn't change the system if it's already in the desired state.

## Key Features

- Declarative language (YAML)
- Inventory management (static or dynamic)
- Role-based playbook structure for reusability
- Built-in modules for common tasks (e.g., copy, file, package, service)
- Extensible with custom modules and plugins

## Real-World Use Cases

- Provision EC2 instances in AWS
- Install and configure Docker on Linux VMs
- Deploy and manage Kubernetes clusters
- Automate patching and package updates
- Push application code and restart services

## Architecture Overview

- **Control Node:** The system where Ansible is installed.
- **Managed Nodes:** The systems being configured (SSH access required).
- **Inventory:** A list of managed nodes (can be IPs, DNS names, etc.)
- **Playbooks:** YAML files with tasks and roles.
- **Modules:** Reusable units of code used in tasks (e.g., `yum`, `apt`, `service`).

## Common Terminology

| Term         | Description |
|--------------|-------------|
| Playbook     | A YAML file with one or more plays (set of tasks) |
| Task         | A single action (e.g., install package, start service) |
| Module       | A unit of work to be executed by a task |
| Inventory    | Hosts or groups of hosts to manage |
| Role         | A way to organize playbooks into reusable components |
| Handlers     | Tasks triggered by `notify` keyword (e.g., restart service) |
| Facts        | System information collected automatically |

## Summary

Ansible makes infrastructure automation simple, repeatable, and secure by leveraging SSH and declarative YAML syntax. It is widely used in DevOps pipelines and is ideal for provisioning, configuration, orchestration, and continuous deployment.

