# Resource Justification – Article CMS Deployment

This document evaluates two Azure deployment options for the Article CMS web application:  
**Azure Virtual Machine (VM)** vs **Azure App Service**.  
The analysis includes cost, scalability, availability, workflow, and overall suitability.

---

## Option 1 – Azure Virtual Machine (VM)

### Cost
- You pay for the VM **24/7**, even when the app is idle.
- Additional cost for disk storage, bandwidth, backup, and monitoring.
- Example Cost:
  - Standard B1ls VM in East US: **~$5–$10/month**
  - To ensure high availability (2+ VMs + Load Balancer): **$30–$50+/month**

### Scalability
- **Manual scaling**
  - Resize VM = downtime
  - Scale out = configure load balancer + manage multiple VMs
- More maintenance effort during high traffic periods

### Availability
- You must design your own HA environment:
  - Availability sets or zone redundancy
  - Manual failover handling
- If a single VM fails → App downtime

### Deployment Workflow
- Manual configuration:
  - SSH into server
  - Configure Python, Flask, dependencies, security patches
- Deployment requires manual Git pulls, restarting services
- **High operational overhead**, but full control of OS

---

## Option 2 – Azure App Service (Web App)

### Cost
- Pricing based on App Service Plan, not runtime hours
- Several features included: logging, SSL, platform patching
- Example Cost:
  - Free Tier (F1) for testing: **$0**
  - Basic Tier (B1): **~$9–$20/month** with scaling and SLA

### Scalability
- **Automatic horizontal scaling**
- Azure manages instances and traffic distribution
- Zero-downtime scaling in supported tiers

### Availability
- Built-in redundancy: Azure replaces unhealthy instances automatically
- Microsoft patches OS/runtime — fewer service failures
- Higher tiers offer SLA-backed uptime guarantees

### Deployment Workflow
- Integrated CI/CD from GitHub
- App settings configured in Azure Portal
- No need to manage OS or infrastructure
- **Much simpler**, more DevOps-friendly workflow

---

## Comparison

| Attribute     | Azure VM | Azure App Service |
|--------------|----------|------------------|
| Cost | Pay per VM 24/7 + infrastructure | Cheaper for small/medium apps, included platform features |
| Scalability | Manual scaling, downtime likely | Auto-scaling built-in |
| Availability | You must design redundancy | Platform redundancy provided |
| Workflow | Full OS control, manual deployments | Automated deployments, minimal admin |

---

## Final Choice: **Azure App Service**

Azure App Service is the best fit for this project because:

- It requires **lower operational effort** than a VM
- It offers **auto-scaling**, load balancing, and redundancy **by default**
- GitHub integration simplifies continuous deployment
- Cheaper and easier to manage for this small CMS application

A VM would provide deeper control of the OS and environment, but that level of control is not required for this project and would increase maintenance costs and complexity.

---

## When Would I Change This Decision?

I would consider switching to a **Virtual Machine** if:

- The app required **custom OS configuration** or unsupported frameworks
- Advanced networking or security customizations were needed
- The application required **dedicated compute hardware** for heavy workloads
- We needed specialized server-level controls not offered by App Service

In those scenarios, a VM would provide better flexibility despite the higher operational burden.

---

### Conclusion

For the current Article CMS project, **Azure App Service is the most cost-effective, scalable, and operationally simple solution** while still providing strong reliability and performance.
