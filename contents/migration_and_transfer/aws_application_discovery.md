# AWS Application Discovery Service

AWS Application Discovery Service is a powerful tool designed to streamline the planning and execution of migrations to the AWS cloud. By collecting detailed usage and configuration data from on-premises servers and databases, it provides insights that are critical for a smooth and efficient migration process.

With its integration into **AWS Migration Hub**, the Application Discovery Service enhances migration tracking by offering a centralized platform where users can visualize discovered servers, group them into applications, and monitor migration progress. This centralization simplifies the process and ensures a well-orchestrated migration. For database workloads, **AWS Database Migration Service Fleet Advisor** complements this process by evaluating options thoroughly, ensuring informed and strategic decision-making.

## Discovery Approaches

AWS Application Discovery Service offers two distinct methods to perform discovery and collect data about on-premises servers:

### 1. Agentless Discovery

The **Agentless Discovery** method utilizes the **Application Discovery Service Agentless Collector** deployed in a **VMware vCenter** environment. This method eliminates the need for installing agents on individual virtual machines (VMs), simplifying the discovery process.

- **Capabilities**:
  - Identifies virtual machines and associated hosts within the vCenter environment.
  - Collects **static configuration data**, such as:
    - Server hostnames
    - IP addresses
    - MAC addresses
    - Disk resource allocations
    - Database engine versions and schemas
  - Gathers **utilization metrics**, including average and peak values for:
    - CPU
    - RAM
    - Disk I/O
  - Supports the discovery of database and analytics servers, enabling inventory collection and performance metric analysis without requiring installation on individual servers.

This method is ideal for environments where minimal disruption and fast deployment are priorities.

### 2. Agent-Based Discovery

The **Agent-Based Discovery** method uses the **AWS Application Discovery Agent** installed directly on each VM or physical server. This approach provides more detailed insights compared to agentless discovery, capturing in-depth performance and system data over time.

- **Capabilities**:
  - Captures **static configuration data**.
  - Collects **detailed performance metrics**, such as:
    - CPU and memory utilization
    - Disk usage
    - Running processes
  - Tracks **inbound and outbound network connections**, offering a comprehensive view of interdependencies and network traffic patterns.

This method is suitable for detailed migration planning and performance optimization, especially for complex environments requiring fine-grained visibility.

## Use Cases

### **Agentless Discovery**

- Best suited for organizations with virtual machines in a VMware vCenter environment.
- Quickly collects system information by deploying an on-premises appliance within vCenter.
- Provides a broad inventory of VMs, hosts, and their configurations without requiring extensive installations.

### **Agent-Based Discovery**

- Ideal for environments where detailed insights into system performance, running processes, and network dependencies are required.
- Supports **Windows** and **Linux** systems, making it versatile for diverse server environments.

## Key Benefits and Features

### Migration Planning

AWS Application Discovery Service facilitates effective migration planning by providing comprehensive insights into on-premises data centers, enabling organizations to:

- Understand **server utilization** and dependencies.
- Optimize migration strategies based on performance data.

### Centralized Data Management

Through integration with **AWS Migration Hub**, all discovery data can be viewed in one place, allowing teams to:

- Visualize discovered servers and group them into applications.
- Monitor migration progress in real-time.

### Versatility in Data Collection

- **Agentless Discovery** focuses on VMware environments, offering a rapid, low-impact solution for gathering key metrics.
- **Agent-Based Discovery** delves deeper, capturing granular details for environments requiring fine-tuned migration plans.

### Comprehensive Insights

- Collects and analyzes historical and real-time **performance metrics**, including:
  - CPU, memory, and disk usage for VMs.
  - System configuration and network connection details for physical servers.

## Integration with AWS Migration Hub

By integrating with **AWS Migration Hub**, the Application Discovery Service simplifies migration management. Users gain:

- A **centralized view** of all discovered assets and applications.
- The ability to **group resources** and monitor their migration status within the console.
- Seamless tracking of both discovery and migration activities in their home AWS region.
