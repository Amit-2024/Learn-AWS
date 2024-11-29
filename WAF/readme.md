# AWS Web Application Firewall (WAF) Guide

AWS Web Application Firewall (WAF) helps protect web applications from common web exploits and bots that may affect availability, compromise security, or consume excessive resources. This guide provides an overview, configuration steps, and a real-world use case for AWS WAF.

---

## Features

- **Customizable Rule Sets**: Create rules to filter web traffic based on IP addresses, HTTP headers, payloads, and more.
- **Integration with AWS Services**: Works seamlessly with Amazon CloudFront, API Gateway, and Application Load Balancer (ALB).
- **Bot Control**: Block or allow access to web traffic originating from known bots.
- **AWS Managed Rules**: Use pre-configured rules for common vulnerabilities, such as OWASP Top 10 threats.
- **Logging and Monitoring**: Integrates with Amazon CloudWatch and Kinesis for detailed traffic analysis.

---

## Use Case: Protecting an E-Commerce Website

### **Scenario**
An e-commerce website deployed using **Amazon CloudFront** as a CDN (Content Delivery Network) faces potential threats such as SQL injection, cross-site scripting (XSS), and bot traffic. AWS WAF is used to:
1. Block malicious requests targeting the application.
2. Allow only trusted IPs for administrative access.
3. Mitigate bot traffic to reduce resource strain.

### **Solution**
AWS WAF is deployed with the following rules:
- **Managed Rule Groups**: Includes AWS-managed rules for SQL injection and XSS protection.
- **IP Block/Allow List**: Blocks suspicious IP ranges detected by external threat intelligence feeds.
- **Rate-Based Rules**: Limits excessive requests from specific IPs to prevent DDoS attacks.
- **Bot Control**: Blocks or challenges known bot traffic.

---

## Workflow

1. **Create a Web ACL**:
   - A Web Access Control List (Web ACL) acts as the primary control structure for WAF rules.
   - Add rules and rule groups to filter incoming requests.

2. **Associate Web ACL with Resources**:
   - Link the Web ACL to an Amazon CloudFront distribution, API Gateway, or ALB.

3. **Define and Apply Rules**:
   - Create custom or use AWS-managed rules to define traffic filtering logic.

4. **Monitor and Tune**:
   - Use Amazon CloudWatch logs to analyze web traffic and fine-tune WAF rules.

---

## Real-World Example: Securing a Blog Site

### Scenario
A popular blog hosted on an **Application Load Balancer** (ALB) faces a sudden spike in bot traffic attempting to scrape content and overload the server.

### Solution with AWS WAF:
1. **Create a Web ACL**:
   - Add AWS Managed Rules to block XSS and SQL injection attacks.
   - Use rate-based rules to block IPs making more than 100 requests per minute.

2. **Add a Custom Rule**:
   - Allow requests from known safe IPs (e.g., admins) and block all others attempting access to sensitive paths like `/admin`.

3. **Associate the Web ACL**:
   - Attach the Web ACL to the ALB to inspect incoming traffic.

4. **Monitor Logs**:
   - Use CloudWatch logs to analyze blocked requests and adjust rules dynamically.

---

## Example Configuration

### Creating a Web ACL via AWS CLI
```bash
aws wafv2 create-web-acl \
  --name "ECommerceWebACL" \
  --scope "CLOUDFRONT" \
  --default-action '{"Allow": {}}' \
  --rules '[
    {
      "Name": "AWS-AWSManagedRulesCommonRuleSet",
      "Priority": 1,
      "Statement": {
        "ManagedRuleGroupStatement": {
          "VendorName": "AWS",
          "Name": "AWSManagedRulesCommonRuleSet"
        }
      },
      "Action": {
        "Block": {}
      },
      "VisibilityConfig": {
        "SampledRequestsEnabled": true,
        "CloudWatchMetricsEnabled": true,
        "MetricName": "AWSManagedRulesCommonRuleSet"
      }
    }
  ]' \
  --visibility-config '{"SampledRequestsEnabled": true,"CloudWatchMetricsEnabled": true,"MetricName": "ECommerceWebACL"}' \
  --region us-east-1
