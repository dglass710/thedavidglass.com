---
title: "Azure Cloud Architecture Project: Secure Web Application Deployment"
date: 2024-03-28
slug: "azure-cloud-architecture-project-secure-web-application-deployment"
type: post
categories: ["projects"]
---

## Project Overview

I designed and deployed a secure web application architecture on Microsoft Azure. The setup used a jump box provisioner with Ansible to automate configuration management and deploy Docker containers across three web servers.

## Architecture Details

- **Jump Box Provisioner:** Served as the single entry point for administrative tasks and ran Ansible to automate deployment across all three web servers.
- **Ansible Configuration Management:** I ran Ansible from the jump box to automate deployment and management of web servers running Docker containers. This kept configurations consistent across all servers.
- **Docker Containers:** Each web server hosted Docker containers running the web application in isolated environments.
- **Load Balancer:** I placed a load balancer in front of the three web servers to distribute incoming traffic on ports 80 and 443, keeping the servers themselves off the public internet with no direct inbound routes.
- **Network Security Groups (NSGs):** Controlled inbound and outbound traffic within the Azure network. I configured NSGs to allow SSH on port 22 only from my home network IP to the jump box, and to permit public HTTP/HTTPS traffic only through the load balancer.

## Security Design

The layered approach — restricted jump box access, isolated Docker containers, and tightly scoped NSG rules — meant that a compromised web server couldn't reach the jump box or other web servers directly. The NSGs blocked all inter-VM SSH traffic except from the jump box, so there was no path from one web server to another without going through the provisioner first.

## Challenges

I locked myself out of the jump box twice while tightening the NSG rules. The problem was rule priority ordering — my deny-all rule was evaluating before the allow-SSH rule, so my own IP got blocked. I diagnosed it by checking the NSG effective rules in the Azure portal, which showed the deny hitting first. The fix was reordering the rule priorities so the allow-SSH rule evaluated before the broad deny.

## Outcome

The final architecture — one jump box, three web servers, a load balancer, and scoped NSG rules — maintained a clear security boundary at each layer with no public SSH exposure and no direct routes between web servers.
