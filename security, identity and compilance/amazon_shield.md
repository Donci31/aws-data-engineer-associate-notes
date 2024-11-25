# Amazon Shield

Amazon Shield is a managed Distributed Denial of Service (DDoS) protection service designed to safeguard AWS resources against network and application layer attacks. There are two levels of AWS Shield protection: **Shield Standard** and **Shield Advanced**. Both services help protect AWS customers from large-scale, sophisticated DDoS attacks, ensuring that applications and services remain available during potential disruptions.

## Key Features of Amazon Shield:

1. **AWS Shield Standard**:
   - **Free Service**: Shield Standard is automatically enabled for all AWS customers and offers protection against common DDoS attacks without additional cost.
   - **Network and Transport Layer Protection**: Shield Standard provides protection against basic attacks like SYN/UDP Floods, Reflection attacks, and other layer 3 and layer 4 attacks.
   - **Automatic Protection**: Shield Standard automatically protects all AWS resources (e.g., EC2, ELB, CloudFront, and Route 53) from these common threats without needing to take further action.

2. **AWS Shield Advanced**:
   - **Paid Service**: Shield Advanced offers additional, more robust protection for customers who need enhanced security against larger and more sophisticated DDoS attacks. It costs $3,000 per month per organization.
   - **Comprehensive Protection**: Shield Advanced protects against attacks targeting Amazon EC2, Elastic Load Balancing (ELB), Amazon CloudFront, AWS Global Accelerator, and Amazon Route 53. It goes beyond Shield Standard’s protection by offering defense against more sophisticated attacks on the application layer (Layer 7).
   - **24/7 Access to the DDoS Response Team (DRT)**: Customers can access AWS's DDoS Response Team for guidance on mitigating attacks and improving security posture.
   - **Protection from DDoS-related Usage Spikes**: Shield Advanced also covers additional charges that may arise due to usage spikes during a DDoS attack, such as increased traffic or resource consumption.
   - **Application Layer Mitigation**: Shield Advanced works with **AWS WAF (Web Application Firewall)** to provide automatic application layer (Layer 7) DDoS mitigation. This feature evaluates and deploys AWS WAF rules to defend against malicious web traffic that targets your applications.

3. **Multi-Account Support**:
   - Shield Advanced can be enabled on multiple AWS accounts under a single consolidated billing umbrella. Once enabled on each account via the AWS Management Console or API, it will provide DDoS protection across all linked AWS accounts, streamlining the management of DDoS defense across large environments.

## Key Use Cases:

1. **Protection for EC2, ELB, CloudFront, Global Accelerator, and Route 53**:
   - Shield Advanced provides additional layers of protection for AWS resources, including EC2 instances, load balancers, and global distribution points like CloudFront and Route 53, ensuring that they remain resilient to large-scale DDoS attacks.

2. **Enhanced Web Application Protection**:
   - By using **AWS WAF** alongside Shield Advanced, customers can create custom rules to filter and block malicious web traffic, safeguarding web applications from sophisticated Layer 7 attacks such as SQL injection, cross-site scripting (XSS), and bot-driven attacks.

3. **Disaster Recovery and Availability**:
   - With near real-time visibility into attacks, Shield Advanced enables quick responses to mitigate DDoS attacks, ensuring that critical applications remain online and operational.

4. **Cost Protection During Attacks**:
   - Shield Advanced helps mitigate the financial impact of DDoS attacks by covering increased resource costs that might result from traffic spikes or higher usage associated with attack mitigation.
