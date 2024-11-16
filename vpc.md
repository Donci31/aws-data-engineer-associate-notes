#  Virtual Private Cloud (VPC)
Amazon Virtual Private Cloud (VPC) is a fundamental service that allows you to create a logically isolated network within the AWS cloud. It enables the definition of your own IP address ranges, subnets, routing tables, and network gateways. A VPC is designed to isolate your AWS resources from other customers, providing control over network traffic and ensuring security within your environment.

## Subnets
subnets are used to divide a VPC into smaller network segments. Each subnet is associated with a specific availability zone (AZ) in a region, and can either be a public subnet or a private subnet

- **Public subnet**: A public subnet is a subnet that is configured to allow communication between resources in the subnet and the internet. Instances placed in a public subnet can have public IP addresses and are typically used for resources that need to be accessed from the internet, such as web servers or load balancers.
    - Use cases: Web servers, Load Balancers.
- **Private subnet**: A private subnet is a subnet that is isolated from direct access to the internet. Instances in a private subnet cannot communicate with the internet unless configured with a NAT Gateway or NAT Instance in a public subnet.
    - Use cases: Databases, backend services.

### **IPv6 and VPC CIDR Blocks**
By default, VPCs use IPv4 addresses, but they can be configured for dual-stack mode, allowing both IPv4 and IPv6. This mode gives you the flexibility to use IPv4 for legacy applications while supporting IPv6 for newer deployments. 

In terms of IP addressing, VPC CIDR blocks must be selected from a specific range. The smallest block is a `/28` (16 IP addresses), and the largest is a `/16` (65,536 IP addresses). These limits ensure that your VPC can scale based on the number of resources needed.


For instance, if a company’s application in one VPC needs to access an ECS cluster in another VPC while adhering to a strict policy of no internet exposure, the correct solution is to use AWS PrivateLink in a service provider model. In this setup, the ECS-hosting VPC acts as the service provider and deploys an NLB, while the application-hosting VPC acts as the consumer and connects through a PrivateLink interface endpoint.

## **VPC Endpoint**
A VPC endpoint enables private connectivity between a VPC and AWS services or privately hosted applications without exposing traffic to the public internet. This feature is powered by AWS PrivateLink and greatly simplifies network architecture while enhancing security.

There are two types of VPC endpoints. The first type, interface endpoints, uses Elastic Network Interfaces (ENIs) within a subnet as entry points for traffic to AWS services or services hosted by other accounts. These endpoints are charged based on the number of hours they are active and the data they process. The second type, gateway endpoints, is used exclusively for **Amazon S3 and DynamoDB**. These endpoints are added to route tables and incur no additional charges, providing reliable connectivity without requiring NAT devices.

## **Internet Gateway**
An Internet Gateway (IGW) facilitates communication between a VPC and the internet. It acts as a target in route tables for internet-bound traffic and performs Network Address Translation (NAT) for instances with public IPs. It supports both IPv4 and IPv6 traffic, and its horizontally scaled architecture ensures high availability without introducing bandwidth constraints or availability risks.

To enable internet access for resources within a VPC, an IGW must be attached to the VPC, and a route to the IGW must be added in the subnet’s route table. Subnets associated with a route table that includes a route to an IGW are considered public subnets, while those without such a route are private subnets. Additionally, instances in the public subnet must have globally unique IP addresses, such as public IPv4 addresses or Elastic IPs, and the associated security group and network ACLs must allow relevant traffic.

## **NAT Gateway**
A NAT Gateway allows instances in private subnets to access the internet or other AWS services while blocking incoming connections initiated from the internet. A NAT Gateway is created in a public subnet and is associated with an Elastic IP address. After creation, the route tables of private subnets must be updated to direct outbound internet traffic to the NAT Gateway.

Unlike NAT instances, NAT Gateways are fully managed by AWS, ensuring scalability and reliability. NAT Gateways support port forwarding and can have security groups associated with them. While NAT Gateways are cost-effective and scalable, they incur charges based on hourly usage and data processing.

NAT Instances, on the other hand, require manual management and maintenance, which makes them less ideal for most use cases. For IPv6 traffic, Egress-Only Internet Gateways should be used instead of NAT.

## **VPC Peering Connection**
A VPC peering connection provides a private networking link between two VPCs, allowing them to route traffic using private IP addresses. However, VPC peering does not support transitive peering. For example, if VPC A is peered with VPC B and VPC C, there will be no direct communication between VPC B and VPC C through VPC A. To overcome such limitations, AWS Transit Gateway can be used. It acts as a central hub for interconnecting multiple VPCs and on-premises networks, simplifying complex network architectures.

## **VPC Sharing**
With VPC sharing, enabled through AWS Resource Access Manager, the owner of a VPC can share subnets with other AWS accounts within the same AWS Organization. Participants can use the shared subnets to create and manage their resources but cannot view or modify resources belonging to other participants.

## **VPC Flow Logs**
VPC Flow Logs enable the capture of detailed information about the traffic to and from resources within a VPC. For instance, capturing flow logs for an Amazon RedShift cluster can help monitor data transfer operations like COPY and UNLOAD commands. These logs can be stored in an Amazon S3 bucket for analysis using Amazon Athena, providing comprehensive insights into network activity and data movement.

## **VPN Connection**
A VPN connection creates a secure tunnel between on-premises environments and the AWS cloud. This ensures the confidentiality and integrity of data transferred between the two environments. To further enhance security, JDBC with SSL encryption can be used for database connections, ensuring secure data transmission over the VPN.

## **Security Groups**

A **Security Group** acts as a virtual firewall for instances to control inbound and outbound traffic. It is used to protect Amazon EC2 instances and other AWS resources by defining rules that control the allowed traffic.

- **Stateful**: Security groups are stateful, meaning if a request is allowed in, the response is automatically allowed out, regardless of inbound or outbound rules.
- **Rules**: Security groups have only **allow** rules. You cannot create a rule that denies traffic; all denied traffic is implicitly dropped.
- **Default Behavior**: By default, a security group allows all outbound traffic, but no inbound traffic unless explicitly specified.
- **Association**: You can assign one or more security groups to an instance, and the rules of all associated security groups are evaluated to determine whether traffic is allowed.
- **Modification**: Security group rules can be modified at any time, and changes are automatically applied to all instances associated with the security group.

On the other hand, a **Network Access Control List (ACL)** is an additional layer of security that acts at the subnet level, controlling inbound and outbound traffic for all resources within that subnet.

### **Key Differences:**

| Feature                  | **Security Groups**                                | **Network ACLs**                              |
|--------------------------|----------------------------------------------------|------------------------------------------------|
| **Scope**                | Instance-level                                    | Subnet-level                                  |
| **Stateful/Stateless**   | Stateful (automatically tracks connection state)   | Stateless (must allow both inbound and outbound traffic) |
| **Rules**                | Only **allow** rules                               | Both **allow** and **deny** rules              |
| **Default Behavior**     | Allows all outbound, no inbound unless specified   | Allows all inbound and outbound by default    |
| **Traffic Evaluation**   | Evaluates all rules from all associated security groups | Evaluates rules based on order of evaluation (first match wins) |
| **Modification**         | Automatically applies changes to all instances     | Must be updated separately for each subnet    |
| **Use Case**             | Used to control traffic to individual instances    | Used for additional layer of subnet-level security |
