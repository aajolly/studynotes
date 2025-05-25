# Google Cloud Monitoring
## Core Concepts
1. **Metrics Scope**
    - A metrics scope is the root entity for monitoring configuration.
    - It can include up to 375 monitored projects and optionally AWS accounts.
    - The scoping project defines the name and access control for the scope.
    - All users with access to the scope can view all monitored project data—consider separate scopes for different visibility needs.
2. **Dashboards & Charts**
    - Visualize metrics like CPU, network traffic, and custom application metrics.
    - Use filters, groups, and aggregations to reduce noise and improve clarity.
3. **Alerting Policies**
    - Trigger alerts based on thresholds (e.g., network egress > X for Y minutes).
    - Notify via email, SMS, or Pub/Sub.
    - Best practices:
        - Alert on symptoms, not causes.
        - Use multiple notification channels.
        - Customize alerts for the audience.
        - Avoid alert fatigue by tuning thresholds.
4. **Uptime Checks**
    - HTTP, HTTPS, or TCP checks from global locations.
    - Can monitor App Engine, Compute Engine, URLs, or AWS resources.
    - Failures are logged if no response within timeout (e.g., 10 seconds).
5. **Ops Agent**
    - Installed on Compute Engine VMs to collect system and application metrics.
    - Required for memory usage and third-party app monitoring.
    - Supports major OSes: Ubuntu, CentOS, Windows.
6. **Custom Metrics**
    - Push application-specific metrics (e.g., active users) into Monitoring.
    - Useful for autoscaling, anomaly detection, and business KPIs.

## Internal Best Practices & Observability Insights
From internal files like Versent - APA CloudOps and Cover-more - Migration phase 3.02 - Copy, several key practices emerge:
- **Automate observability**: Use Infrastructure as Code to deploy monitoring agents and dashboards 1 2.
- **Alert tuning**: Regularly review and refine alert thresholds to reduce noise 2.
- **Governance loops**: Include observability KPIs in stakeholder reviews and steering committees 2.
- **AI-Ops**: Explore predictive analytics and anomaly detection to proactively manage incidents 2.

## AWS CloudWatch Comparison
| **Feature** | **Google Cloud Monitoring** | **Amazon CloudWatch** |
| :-------- | :---------- | :--------- |
| **Metrics Scope** | Multi-project, multi-cloud | Per AWS account/region |
| **Dashboards** | Custom, real-time, multi-project | Custom, per account |
| **Alerting** | Policies with multiple channels | Alarms with SNS, Lambda |
| **Uptime Checks** | Global checks, integrates with alerting | Synthetic monitoring (Canaries) |
| **Custom Metrics** | Push via API or Ops Agent | PutMetricData API |
| **Agent** | Ops Agent (system + app metrics) | CloudWatch Agent |
| **Pricing** | $0.01 per 1,000 read API calls | $0.30 per high-res alarm/month 3 |