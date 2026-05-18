---
title: WaveMaker Enterprise Prerequisites
last_update: { author: "Imtiyaz Mohammad" }
id: prerequisites
sidebar_label: Prerequisites
---
You can set up WaveMaker Enterprise on any machine.

:::note
This document uses words like **VM**, **Instance** to refer a machine.
:::

## WME setup system requirements

WaveMaker Enterprise AI can be installed on any machine that meets the following requirements. Before you start setting up WaveMaker Enterprise AI, review the minimum and recommended system requirements for each instance type.

### WME Platform Instance

| Requirement | Minimum configuration |
| ----------- | --------------------- |
| Memory | 32 GB |
| CPU | 8-core, single CPU system |
| Hard disk | 450 GB minimum. For volume-based setups, allocate 100 GB for `/`, 200 GB for `/wm-data`, and 150 GB for `/wm-runtime`. |
| Host OS | Ubuntu 22.x LTS or RHEL 8.x/9.x, kernel 4.4 or later, x86 architecture |
| Software | Docker 28.x, Python 3.5 or later, `wget`, and `jq` |
| Network | Static IP with valid DNS. Open ports 80, 443, 8080, and 22 for SSH access from the developer network range. |

### WME StudioWorkspace Instance and AppDeployment Instance

| Requirement | Minimum configuration |
| ----------- | --------------------- |
| Memory | 32 GB |
| CPU | 8-core, single CPU system |
| Hard disk | 300 GB minimum. For volume-based setups, allocate 100 GB for `/` and 200 GB for `/data`. |
| Host OS | Ubuntu 22.x LTS or RHEL 8.x/9.x, kernel 4.4 or later, x86 architecture |
| Software | Docker 28.x, Python 3.5 or later, `wget`, and `jq` |
| Network | Static IP. Open the required ports for access from the Platform Instance. |

## Port requirements

Open the following ports on the Platform Instance for access from the StudioWorkspace Instance and AppDeployment Instance.

| Port | Required for |
| ---- | ------------ |
| 5000 | Platform services |
| 8500 | Service discovery |
| 22 | SSH access |
| 8081 | Platform communication |
| 2200 | Container SSH access |
| 8100 | StudioWorkspace and AppDeployment communication |
| 9200 | Search and observability services |
| 8000-8020 | Platform-managed application services |
| 8094 | AI service communication |
| 8079 | AI service communication |
| 5432 | Database connectivity |
| 5433 | Vector database access for AI features |
| 8083 | AI Studio and agent-server LiteLLM proxy communication |
| 8086 | AI Studio and agent-server key management |

Open the following ports on the StudioWorkspace Instance and AppDeployment Instance for access from the Platform Instance.

| Port | Required for |
| ---- | ------------ |
| 22 | SSH access |
| 2375 | Docker API access |
| 80 | HTTP access |
| 5000 | Platform service communication |
| 8100 | StudioWorkspace and AppDeployment communication |
| 8888 | Workspace service communication |
| 9101, 9102, 9100 | Metrics collection |
| 9404 | Metrics export |
| 2200-2299 | Container SSH access |
| 8001-8099 | Platform-managed application services |
| 3300-3399 | Database and service communication |
| 9500-9599 | Platform-managed service communication |
| 3000 | Routing traffic from the load balancer to AI Studio |
| 3001 | Routing traffic from the load balancer to AI Studio NGINX |
| 3002 | Routing traffic from the load balancer to agent-server |
| 5010 | Backend MCP |
| 5020 | UI MCP |

### Network Communication

- The following diagram explains the network communication between the Platform Instance, StudioWorkspace Instance, and AppDeployment Instance.

[![network-communication-between-instances](./assets/images/network-communication-between-instances.jpg)](./assets/images/network-communication-between-instances.jpg)

### Capacity planning

Adding an instance to either User Workspace or Deployed Apps increases WME setup capacity for application development and deployment, respectively. Each added User Workspace or Deployed Apps instance supports a specific number of app developments and app deployments. These numbers vary based on the WME version.

| Application Type    | Developer logins per 32GB WaveMaker Studio Instance | 
| ------------------- | --------------------------------------------------- |
| WEB                 | 18           				            |
| App-Preview-ESBuild | 18          					    |
| App-Preview-expo    | 4           					    |


| App Deployments per 32GB WaveMaker AppDeployment Instance |
| --------------------------------------------------------- |
| 20                                                        |


The actual app development and deployment support is also determined by your license terms. Even if your infrastructure has the capacity, the apps that can be developed and deployed are restricted by your license terms. Similarly, even when your license terms allow more apps, the apps that can be developed and deployed are limited by infrastructure capacity.

:::note
Different instances need to be added to each stage in the release pipeline.
:::

## WME Setup Artifacts

WaveMaker will share the required artifacts (installer files/Images) to do the setup. There are two ways to do the setup.

1. **Operating System Pre-Installed**.  
    You can come up with machines with the Operating system pre-installed and install Prerequisite(optional).
    Then use our installer to setup WME.

## IP Addressing and DNS Mapping

You will be needing IP Addresses for the following.

### IP Address

- One static IP for accessing the platform machine from your developer's network.
- Machine Static IP: This is the IP assigned to the machine during setup and should be accessible on your network, or
  - In the case of VM, it will be the local IP address, which should be rout table from in your LAN.
  - In case of AWS instance: Private static IP for the instance within your VPC (assigned via eth0 or via ENI on eth1,ens5)

### DNS Mapping

Map a domain to the above IP for easy access.

| **Domain**              | **Domain URL**                                                                      | **Description**                                                                          |
| ----------------------- | ----------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| WaveMaker Studio        | `wavemaker.[mycompany].com`                                                         | This domain will be used to access WaveMaker Studio                                      |
| WaveMaker Deployed Apps | `wm-apps.[mycompany].com`  `wm-stage.[mycompany].com`     `wm-live.[mycompany].com` | These domains will be used to access WaveMaker Studio apps deployed onto WaveMaker Cloud |

:::note
In the preceding table, `[mycompany]` is used as an example. You may have to replace `[mycompany]` with your appropriate domain name.
:::

### Docker Container Access

- An IP range to be assigned to the Docker containers internally. The Minimum [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#CIDR_notation) (Classless Inter-Domain Routing) range for Docker container network is 24.

You will be needing to assign a /24 CIDR to Docker during setup. This IP range should not be in use anywhere on your network and can be completely different from your network’s range. These IPs are assigned internally by Docker to containers and these IPs won’t be exposed on your network.

For example, if your network is using a 10.x.x.x_range and the range_192.168.x.x is not used anywhere in your network, you may assign this 192.168.x.x range to Docker. See [here](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces) for the possible LAN IP ranges.
