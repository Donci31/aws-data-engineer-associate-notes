# Amazon Route 53

Amazon Route 53 is a highly available and scalable Domain Name System (DNS) web service provided by AWS. It is designed to translate human-readable domain names such as `www.example.com` into machine-readable IP addresses like `203.0.113.77`. This translation ensures that users can easily access applications hosted on AWS or anywhere else by simply entering a domain name, instead of remembering complex numerical IP addresses.

## Record Types and Features

### CNAME Records

A **CNAME** (Canonical Name) record allows you to map one domain name to another. For instance, you might want to map `acme.example.com` to `example.com` or `zenith.example.org`. However, it's important to note that DNS does not allow creating a CNAME record for the **zone apex**, which is the top node of a DNS namespace (e.g., `example.com` itself).

### Alias Records

**Alias records** are an AWS-specific extension of standard DNS functionality, which allow you to route traffic to AWS resources like **Amazon S3 buckets**, **CloudFront distributions**, or other Route 53 records. One of the key benefits of Alias records is that they can be created at the **zone apex** (the root of your domain), which is not possible with CNAME records.

For example, you cannot create a CNAME record for `example.com`, but you can create an alias record that routes traffic from `example.com` to a CloudFront distribution. Additionally, Route 53 does not charge for queries made to alias records that route traffic to AWS resources, unlike CNAME records, which incur charges.

### A Records

**A records** (Address records) are used to map a domain name to an IPv4 address. For example, an A record could map `www.example.com` to the IPv4 address `203.0.113.25`. These records are essential for routing traffic to resources with an IPv4 address.

### AAAA Records

**AAAA records** are similar to A records but are used to map a domain name to an **IPv6** address. For instance, an AAAA record can be used to map `ipv6.example.com` to the IPv6 address `2001:0db8:85a3:0000:0000:8a2e:0370:7334`. As IPv6 adoption increases, these records are increasingly important for ensuring that resources are accessible over the newer IPv6 network.

## Route 53 DNS Resolver

In each VPC, Amazon provides a default DNS server known as the **VPC DNS server**, which is responsible for translating domain names into IP addresses for both public and private domains. When DNS resolution is enabled in a VPC, Route 53 Resolver helps translate domain names into IP addresses, allowing instances in the VPC to access both internal and external resources.

For private hosted zones, DNS queries can only be resolved by the VPC DNS server. Therefore, enabling both **DNS support** and **DNS hostnames** is a prerequisite to successfully resolving queries for a private hosted zone.

## Types of Hosted Zones

Route 53 allows you to create two types of hosted zones: **Public Hosted Zones** and **Private Hosted Zones**.

1. **Public Hosted Zones** are used for resolving domain names that are accessible over the internet. A public hosted zone is typically used for managing the domain and routing traffic for domains like `example.com` and its subdomains, such as `www.example.com`. Public hosted zones allow DNS queries from anywhere on the internet.

2. **Private Hosted Zones** are used for managing domain names within one or more Amazon Virtual Private Clouds (VPCs). These zones ensure that only resources within the specified VPC(s) can resolve DNS queries for domain names in the private hosted zone. For example, you might create a private hosted zone for an internal domain like `internal.example.com`, which is used for internal applications not accessible over the internet.

For a private hosted zone to work properly, the VPC must be configured with two important DNS settings:
- `enableDnsHostnames`: Ensures that instances in the VPC get automatically assigned DNS hostnames, such as `ec2-203-0-113-25.compute-1.amazonaws.com`.
- `enableDnsSupport`: Allows Amazon’s Route 53 Resolver to resolve DNS queries for both internet domain names and internal VPC resources.

## Health Checks and DNS Failover

Amazon Route 53 offers **DNS Health Checks** to ensure that traffic is routed only to healthy endpoints. If you have multiple resources performing the same function (e.g., multiple web servers or load balancers), you can configure **DNS Failover** to reroute traffic from unhealthy resources to healthy ones. This is critical for building resilient and highly available applications.

- **Active-Passive Failover**: In this configuration, a primary resource handles the majority of traffic, while a secondary resource remains on standby to take over if the primary resource becomes unavailable.
- **Active-Active Failover**: In this configuration, all resources are active and Route 53 uses health checks to determine if any resource is unhealthy. Traffic is routed only to healthy resources, ensuring minimal disruption.

Route 53 integrates with **Elastic Load Balancing (ELB)** to manage health checks for your resources. If any resource behind the ELB becomes unhealthy, Route 53 automatically redirects traffic away from the failed resource.

## Advanced Routing Policies

Amazon Route 53 offers several advanced routing policies to optimize traffic management and ensure high availability across global applications. These policies include:

1. **Latency-Based Routing**: This routing policy directs traffic to the AWS region that offers the lowest latency, ensuring that users experience faster response times when accessing your resources.

2. **Geolocation Routing**: With geolocation routing, you can route traffic based on the geographic location from which the DNS query originates. This can be useful for localizing content or ensuring compliance with data residency requirements.

3. **Geoproximity Routing**: Geoproximity routing allows you to route traffic to resources based on both geographic location and the relative bias you assign to a resource. This can help distribute traffic more evenly or focus traffic on specific regions by adjusting the "bias" value.

4. **Weighted Routing**: This policy lets you define how much traffic is routed to each resource based on a weight value. It is useful for load balancing and can be leveraged to test new versions of applications by routing a small percentage of traffic to the new version while the majority continues to go to the old version.

## TTL (Time to Live)

The **Time to Live (TTL)** setting determines how long DNS resolvers cache a record before querying Route 53 again. A longer TTL can reduce the number of DNS queries, thereby lowering the latency and reducing costs. However, if you need to change a record frequently, a shorter TTL ensures that changes are propagated more quickly across the network.

## DNS Query Forwarding

Route 53 allows integration with external DNS resolvers, enabling DNS queries from your VPC to be forwarded to an on-premises network. You can set up **inbound** and **outbound endpoints** for Route 53 Resolver, allowing queries from your on-premises network to be forwarded to Route 53 and vice versa.

- **Inbound Endpoints**: These allow DNS queries from on-premises DNS resolvers to be forwarded to Route 53.
- **Outbound Endpoints**: These allow DNS queries from Route 53 to be forwarded to external DNS resolvers on your on-premises network.

To configure DNS query forwarding, you can set up **Resolver rules** to specify the domain names to forward (such as `example.com`) and the target DNS resolvers.
