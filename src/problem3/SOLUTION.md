# Diagnose Me Doctor: Troubleshooting High Memory Usage on a VM Running NGINX

This repository documents the steps to troubleshoot high memory usage (99%) on a virtual machine (VM) running an NGINX load balancer on Ubuntu 24.04. The VM is hosted on a cloud-based infrastructure (e.g., AWS), and the goal is to identify the root cause, mitigate the issue, and prevent future occurrences.

---

## **Problem Statement**
A VM running an NGINX load balancer is consistently using 99% of its memory. The VM has 64GB of storage and is responsible for routing traffic to upstream services. The high memory usage is causing performance degradation and potential service outages.

---

## **Troubleshooting Steps**

### **1. Verify Monitoring Data**
- Check CloudWatch (or equivalent monitoring tool) to identify when memory usage spiked.
- Correlate the spike with events like traffic surges, deployments, or configuration changes.
- If the spike is due to a predictable event (e.g., flash sale), plan for scaling solutions like **Auto Scaling Groups (ASG)** or **Elastic Load Balancers (ELB)**.

---

### **2. Investigate NGINX Configuration**
- Check the NGINX configuration file (`/etc/nginx/nginx.conf` or `/etc/nginx/sites-available/default`) for errors.
- Test the configuration using `nginx -t`.
- Review NGINX logs (`/var/log/nginx/access.log` and `/var/log/nginx/error.log`) for unusual patterns or errors.
- Use tools like `nginx-status` or `stub_status` to monitor NGINX metrics.

---

### **3. Diagnose at the OS Level**
- Use commands like `top`, `htop`, or `free -m` to identify memory-hungry processes.
- Check for memory leaks using `vmstat` or `sar`.
- Review system logs (`/var/log/syslog` or `journalctl`) for errors or warnings.

---

### **4. Mitigate and Recover**
- **If NGINX is the culprit:**
  - Optimize NGINX configuration (e.g., `worker_processes`, `worker_connections`).
  - Restart NGINX gracefully: `sudo systemctl restart nginx`.
- **If another process is the culprit:**
  - Terminate the problematic process using `kill` or `pkill`.
- **If memory leak is suspected:**
  - Restart the VM and update software.
- **If traffic surge is the cause:**
  - Implement scaling solutions (e.g., ASG, ELB) or use a CDN.

---

### **5. Prevent Future Issues**
- Optimize VM configuration (e.g., minimal OS image, remove unnecessary packages).
- Set up CloudWatch alerts for high memory usage, CPU usage, and disk I/O.
- Automate scaling using AWS Auto Scaling Groups (ASG).
- Perform regular load testing and continuous monitoring.

---

## **Possible Root Causes and Impacts**
1. **NGINX Misconfiguration:**
   - **Impact:** High memory usage, slow response times, or service downtime.
   - **Recovery:** Optimize NGINX configuration and restart the service.

2. **Traffic Surge:**
   - **Impact:** Overwhelmed server, high memory usage, and potential crashes.
   - **Recovery:** Scale resources horizontally or vertically, and implement traffic offloading.

3. **Memory Leak:**
   - **Impact:** Gradual memory exhaustion, leading to system instability.
   - **Recovery:** Restart the VM, update software, and monitor for recurrence.

4. **Malware or Unauthorized Processes:**
   - **Impact:** Unauthorized resource usage, potential security breaches.
   - **Recovery:** Terminate the process, investigate the source, and secure the VM.

5. **Insufficient Resources:**
   - **Impact:** Inability to handle traffic, leading to high memory usage.
   - **Recovery:** Upgrade VM instance type or optimize resource allocation.

---