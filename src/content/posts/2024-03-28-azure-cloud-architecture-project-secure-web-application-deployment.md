---
title: "Azure Cloud Architecture Project: Secure Web Application Deployment"
date: 2024-03-28
slug: "azure-cloud-architecture-project-secure-web-application-deployment"
type: post
description: "Jump box, three web servers, locked-down NSGs. Locked myself out twice getting the rules right."
categories: ["projects"]
---

## Overview

The constraint was simple: deploy a web application on Azure where no server has public SSH access and no web server can reach any other web server directly. I designed a layered architecture around a single jump box provisioner, three Dockerized web servers behind a load balancer, and tightly scoped Network Security Group rules.

## Architecture

All administrative access routes through the jump box, which serves as both the SSH entry point and the Ansible control node. From there, I automated deployment of Docker containers across three web servers — each running the application in an isolated environment. A load balancer handles incoming traffic on ports 80 and 443, keeping the web servers off the public internet entirely.

The NSGs enforce the security boundaries. SSH on port 22 is allowed only from my home IP to the jump box. The web servers accept HTTP/HTTPS only through the load balancer. All inter-VM SSH traffic is blocked except from the jump box, so there's no lateral path between web servers without going through the provisioner.

## Challenges

I locked myself out of the jump box twice while tightening the NSG rules. The problem was rule priority ordering — my deny-all rule was evaluating before the allow-SSH rule, so my own IP got blocked. I diagnosed it by checking the NSG effective rules in the Azure portal, which showed the deny hitting first. The fix was reordering the rule priorities so the allow-SSH rule evaluated before the broad deny.

## What I'd Change

The architecture works, but I'd add monitoring. There's no alerting on failed SSH attempts to the jump box or unusual traffic patterns through the load balancer. I'd also move the Ansible playbooks into a Git repo with CI so configuration changes are versioned and auditable — running ad-hoc playbooks from the jump box worked for this project, but it doesn't scale.
