---
title: "Azure Cloud Architecture Project: Secure Web Application Deployment"
date: 2024-03-28
slug: "azure-cloud-architecture-project-secure-web-application-deployment"
type: post
categories: ["projects"]
---

## Project Overview

I designed and deployed a secure, scalable web application architecture on Microsoft Azure. The setup used a jump box provisioner with Ansible to automate configuration management and deploy Docker containers across multiple web servers.

## Architecture Details

- **Jump Box Provisioner:** Served as the single entry point for administrative tasks. I configured it to allow SSH access (port 22) only from my home network IP.
- **Ansible Configuration Management:** I ran Ansible from the jump box to automate deployment and management of web servers running Docker containers. This kept configurations consistent across all servers.
- **Docker Containers:** Each web server hosted Docker containers running the web application in isolated environments.
- **Load Balancer:** Sat in front of the web servers to distribute incoming traffic. I configured it to handle ports 80 (HTTP) and 443 (HTTPS) for public access.
- **Network Security Groups (NSGs):** Controlled inbound and outbound traffic within the Azure network. I configured NSGs to allow only my home network to SSH into the jump box on port 22 and to permit public HTTP/HTTPS traffic through the load balancer.

## Security and Efficiency

The layered approach — restricted jump box access, isolated Docker containers, and tightly scoped NSG rules — meant that even if one component was compromised, lateral movement was limited. The load balancer provided availability while keeping the web servers themselves out of direct public reach.

## Outcome

The architecture handled traffic scaling with minimal downtime while maintaining a clear security boundary at each layer. The hardest part was getting the NSG rules right without accidentally locking myself out of the jump box during testing — which happened twice before I nailed down the correct rule ordering.
