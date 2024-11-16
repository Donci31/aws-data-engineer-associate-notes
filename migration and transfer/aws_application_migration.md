# AWS Application Migration Service (MGN)

AWS Application Migration Service (MGN) is a highly automated **lift-and-shift (rehost)** solution that simplifies, accelerates, and reduces the cost of migrating applications to AWS. It is designed to seamlessly migrate a large number of **physical, virtual, or cloud servers** without introducing compatibility issues, causing performance disruptions, or requiring long cutover windows.

MGN works by replicating source servers into your AWS account, automatically converting and launching these servers on AWS. Once the migration is complete, organizations can immediately benefit from the cost savings, enhanced productivity, resilience, and agility that come with operating in the cloud. Moreover, after migration, businesses can leverage AWS services to **replatform** or **refactor** their applications, making **lift-and-shift** a fast and effective pathway to modernization.

## Key Features and Capabilities

### **1. Lift-and-Shift (Rehost) Migration**
AWS Application Migration Service specializes in lift-and-shift migrations, providing a streamlined process to migrate applications without requiring code changes or significant reengineering efforts. 

- Simplifies the migration of applications to AWS by rehosting them directly.
- Ensures compatibility with AWS environments while minimizing manual intervention.

### **2. Source Server Replication**
MGN replicates source servers, whether physical, virtual, or cloud-based, into your AWS account. It supports:
- **Automatic server conversion**, ensuring migrated servers run natively on AWS.
- A **wide range of platforms**, operating systems, and databases, making it versatile for various enterprise environments.

### **3. Minimal Downtime and Reduced Costs**
- MGN ensures **minimal disruption** to ongoing operations during migration by maintaining a low cutover window.
- Automates repetitive and time-consuming tasks, reducing the **costs** associated with manual migration processes.

### **4. Post-Migration Modernization**
Once applications are running on AWS, MGN enables organizations to:
- Leverage AWS services for **modernization**.
- **Replatform** or **refactor** applications quickly and efficiently to take full advantage of the cloud's capabilities.

## Comparison with AWS Application Discovery Service

While both AWS Application Migration Service and **AWS Application Discovery Service** support the migration journey, their roles are distinct:

| Feature                         | AWS Application Migration Service (MGN)        | AWS Application Discovery Service                 |
|---------------------------------|------------------------------------------------|--------------------------------------------------|
| **Primary Focus**               | Actual migration of applications and servers   | Planning phase of migrations                     |
| **Purpose**                     | Automates lift-and-shift migrations            | Discovers and collects information about on-premises environments |
| **Capabilities**                | Rehosting applications without compatibility issues | Collecting server specs, network dependencies, and performance data |
| **Output**                      | AWS-hosted servers ready for operation         | Insights for detailed migration planning         |

MGN focuses on the execution phase, ensuring a smooth transition of workloads into AWS, while Application Discovery Service emphasizes the planning phase by gathering critical data.

## Use Cases

### **1. Migrating Legacy Applications**
Organizations with legacy applications can use MGN to rehost these applications on AWS with minimal changes, reducing the risks associated with re-engineering efforts.

### **2. Large-Scale Migrations**
MGN is ideal for enterprises migrating a large number of servers, offering automated tools to simplify the process and reduce errors.

### **3. Disaster Recovery and Resilience**
By moving applications to AWS, businesses can enhance disaster recovery capabilities and resilience while maintaining low downtime during migration.
