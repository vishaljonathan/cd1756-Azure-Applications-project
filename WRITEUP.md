# Resource Justification – Article CMS

## Option 1 – Virtual Machine (VM)

### Cost
- VM size: e.g. Standard B1ls in East US.
- You pay for **compute 24/7**, whether the app is busy or idle.
- You may also pay separately for disk storage, backup, and bandwidth.
- This can be **cheap at small scale** (one small VM), but costs rise if you need multiple VMs for redundancy or scaling.

### Scalability
- Scaling is **manual**: to handle more load you must resize the VM or create additional VMs and configure load balancing yourself.
- Vertical scaling (bigger VM) usually causes **downtime** while resizing.
- Horizontal scaling (more VMs) requires setting up a **load balancer** and managing more instances.

### Availability
- You are responsible for **keeping the VM healthy** (OS patches, security updates, reboots).
- For higher availability you need:
  - at least **2+ VMs** in an availability set or across zones
  - plus a load balancer.
- If the single VM fails, the app is unavailable until you fix it.

### Workflow (Deployment & Operations)
- You manage the **OS**, **runtime**, and **web server** (NGINX / gunicorn, etc.).
- Typical flow:
  1. SSH into VM
  2. Pull code from GitHub
  3. Manage Python virtual environment, dependencies, environment variables
  4. Restart the app / services manually
- More control and flexibility, but **more operational overhead** and room for human error.

---

## Option 2 – Azure App Service (Web App)

### Cost
- Uses a **hosting plan** (e.g. Free F1 or B1/B2 in Basic/App Service Plan).
- You pay for the **App Service Plan**, not per app (multiple apps can share one plan).
- For small to medium apps, App Service is often **cheaper overall** than running and maintaining multiple VMs, especially when you factor in admin time.
- Built-in features (SSL, deployment slots in higher tiers, monitoring) are included in the plan.

### Scalability
- Supports **automatic horizontal scaling** based on rules (CPU, requests, schedule etc. in non-free tiers).
- Scale out and in through Azure Portal or automatically, with **no manual server configuration**.
- No need to manage individual VMs; Azure handles the underlying infrastructure.

### Availability
- The platform provides **built-in redundancy** at the App Service Plan level.
- Azure automatically replaces unhealthy instances and distributes traffic.
- You don’t patch the OS or runtime; Microsoft maintains the underlying environment, which reduces the risk of downtime from misconfiguration.

### Workflow (Deployment & Operations)
- Deployment is integrated with **GitHub Actions** / Azure DevOps:
  - Push to GitHub → pipeline builds and deploys automatically.
- Environment variables are managed through **Application Settings** in the portal.
- Monitoring and logs are available directly from the App Service (Log Stream, Application Insights if enabled).
- Overall, the workflow is **simpler, repeatable, and more “DevOps-friendly”** than logging into a VM.

---

## Comparison and Final Choice

| Aspect       | VM                                           | Azure App Service                            |
|-------------|----------------------------------------------|----------------------------------------------|
| Cost        | Pay per VM 24/7; extra cost for redundancy   | Pay per plan; good value for small/medium apps |
| Scalability | Manual resizing / extra VMs + load balancer  | Built-in scale out/in, no server management  |
| Availability| You design HA with multiple VMs + LB         | Platform provides redundancy automatically   |
| Workflow    | Manual SSH, updates, deployments             | GitHub deployment, portal-based management   |

### Chosen Option: **Azure App Service**

For this CMS project I chose to deploy the application on **Azure App Service** instead of a VM.

Azure App Service gives me **lower operational effort and better scalability** for this small web application. I do not need to manage the underlying OS, patches, or web server configuration, and I can integrate automatic deployments from GitHub. In higher tiers it can automatically scale out to handle more load without manual VM management.  

A VM would give me more low-level control, but for this project that extra control is not necessary and would increase maintenance overhead and complexity. Therefore, App Service is the more appropriate and cost-effective option.

