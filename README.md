# Auto-scaling-with-ALB-using-a-launch-template

---

### **Relevance of Auto Scaling with ALB using a Launch Template**

1. **Launch Template**

   * A **Launch Template** is a blueprint that defines how EC2 instances should be created.
   * It includes details like instance type, AMI ID, key pair, security groups, user data (e.g., startup scripts), and more.
   * This ensures all instances launched (whether manually or by Auto Scaling) are consistent and reproducible.

2. **Auto Scaling Group (ASG)**

   * The ASG uses the Launch Template to **automatically add or remove EC2 instances** based on workload.
   * You define a **minimum, maximum, and desired number of instances**.
   * Scaling policies (like CPU utilization > 70%) trigger scaling out (adding more instances), while low utilization triggers scaling in (removing instances).
   * This ensures **cost efficiency** (you pay only for what you need) and **high availability**.

3. **Application Load Balancer (ALB)**

   * The ALB distributes incoming traffic evenly across the healthy EC2 instances in the Auto Scaling Group.
   * When new instances are launched by Auto Scaling, the ALB automatically detects them (via health checks) and starts routing traffic.
   * If an instance fails, ALB stops sending traffic to it, ensuring fault tolerance.

4. **Why this setup is powerful in DevOps**

   * Ensures **scalability** (app can handle sudden traffic spikes).
   * Provides **resilience** (failed instances replaced automatically).
   * Reduces **manual intervention** (infra self-adjusts).
   * Aligns with **infrastructure as code** practices when managed via IaC tools (CloudFormation, Terraform).

---

### **Testing Auto Scaling with `stress`**

Once your Auto Scaling Group + ALB + Launch Template is set up, you can test how well it scales using the Linux `stress` tool:

1. **Install stress on one of your EC2 instances**:

   ```bash
   sudo yum install -y stress   # Amazon Linux
   # or
   sudo apt-get install -y stress  # Ubuntu/Debian
   ```

2. **Run stress to simulate high CPU load**:

   ```bash
   stress --cpu 4 --timeout 300
   ```

   * `--cpu 4` → spins 4 workers to keep CPUs busy.
   * `--timeout 300` → runs for 300 seconds (5 minutes).

3. **Observe Auto Scaling behavior**:

   * CPU utilization in **CloudWatch** should spike above the threshold (e.g., 70%).
   * The ASG should automatically launch new EC2 instances using the Launch Template.
   * The ALB will start routing traffic to these new instances once they pass health checks.

4. **After stress ends**:

   * CPU usage will drop.
   * The ASG should scale down by terminating extra instances.
   * ALB removes them from routing when they go offline.

---

✅ **Summary**:
Using a **Launch Template with an ASG and ALB** ensures consistent instance configuration, dynamic scaling, and reliable traffic distribution. By using the `stress` tool, you can simulate high load, observe how your infrastructure **auto-scales in real time**, and confirm that your setup is resilient, efficient, and production-ready.

---
