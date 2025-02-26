# AWS WAF – Web Application Firewall

AWS WAF (Web Application Firewall) is a security service designed to protect web applications from common web exploits that may compromise security, affect availability, or consume excessive resources. It operates at **Layer 7** (HTTP/HTTPS) and provides robust protection against malicious traffic targeting web applications.

## Key Features and Deployments

- **Layer 7 Protection**: Defends against attacks such as SQL injection, Cross-Site Scripting (XSS), and other common HTTP-based exploits.
- **Deployment Options**:
  - **Application Load Balancer** (ALB)
  - **Amazon API Gateway**
  - **Amazon CloudFront** (CDN solution)
  - **AppSync GraphQL API**
  - **Cognito User Pool**

## Key Components

- **Web ACL (Web Access Control List)**: Defines a set of rules to filter web traffic based on conditions you specify. You can define rules to allow or block specific traffic patterns.
  - **IP Set**: Allows up to 10,000 IP addresses; multiple rules can be used to handle more IPs.
  - **HTTP conditions**: Includes HTTP headers, HTTP body, or URI strings.
  - **Rate-Based Rules**: Useful for mitigating DDoS attacks by limiting requests based on the rate of incoming traffic.
  - **Geo-Match**: Block traffic from specific countries.
  - **Size Constraints**: Control request sizes to block oversized or abnormal requests.
- **Rule Groups**: Reusable sets of rules that can be added to Web ACLs for consistency and ease of management.
- **Regional Deployment**: AWS WAF is region-specific, except when deployed with **Amazon CloudFront**, which provides global coverage through Edge Locations.

## Usage Scenarios

- **SQL Injection and XSS Protection**: AWS WAF blocks these common attack patterns by analyzing HTTP requests.
- **Geo-blocking**: Block requests from specific countries using geo-match conditions, ensuring traffic is filtered based on geographical origin.
- **Rate Limiting**: Configure rate-based rules to protect against DDoS attacks and malicious traffic surges.

## Integration with AWS Services

- **Amazon CloudFront**: By integrating AWS WAF with CloudFront, security rules are applied at AWS edge locations, reducing the impact of malicious traffic before it reaches your infrastructure.
- **Application Load Balancer**: When integrated with ALB, AWS WAF can protect both public and internal load balancers, filtering malicious requests before they reach your servers.
- **API Gateway**: Protects APIs from common web exploits, safeguarding your backend systems from attacks.

## AWS WAF with AWS Shield and AWS Firewall Manager

- While **AWS WAF** protects against common web attacks, **AWS Shield** is designed for DDoS protection. Using **AWS Shield** with **AWS WAF** offers a comprehensive defense against both Layer 7 web application attacks and DDoS threats.
- **AWS Firewall Manager** simplifies management by allowing you to apply AWS WAF rules across multiple accounts and resources. It also integrates with other security services like **AWS Shield Advanced**, **Amazon VPC Security Groups**, and **AWS Network Firewall**, ensuring a unified security posture across your organization.

## AWS WAF vs AWS Shield

- **AWS WAF**: Protects specifically against Layer 7 attacks such as SQL injection, XSS, and other HTTP-based exploits.
- **AWS Shield**: Focuses on DDoS protection and offers additional mitigation strategies for large-scale, volumetric attacks.
- **Combination**: For optimal security, use AWS WAF in conjunction with AWS Shield Advanced to protect against both application layer attacks and DDoS threats.

| Feature                    | AWS WAF                                        | AWS Shield                                    |
|----------------------------|-----------------------------------------------|-----------------------------------------------|
| **Protection Layer**        | Application Layer (Layer 7)                   | Network & Transport Layers (Layers 3 & 4)      |
| **Main Use**                | Protects against SQL injection, XSS, etc.     | Mitigates DDoS attacks, including large-scale floods |
| **Service Integration**     | CloudFront, ALB, API Gateway, AppSync         | EC2, ELB, CloudFront, Global Accelerator, Route 53 |
| **Cost**                    | Pay-per-request model                         | $3,000 per month for Shield Advanced          |
| **Additional Features**     | Rate limiting, geo-blocking, size constraints | 24/7 access to DDoS Response Team (DRT), Automatic attack mitigation |
| **Combined Use**            | Best used with AWS Shield for full protection  | Best used for large DDoS mitigation, complement with WAF for application layer threats |
