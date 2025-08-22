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
PROJECT TASKS.
1.Create a launch template
  a) on the console, navigate to launch templates on the left
  <img width="946" height="423" alt="image" src="https://github.com/user-attachments/assets/1a1eb6e2-eb1a-4d3a-8ae2-e69f5184506e" />
  b) enter the values for fields on the template section
  <img width="937" height="405" alt="image" src="https://github.com/user-attachments/assets/921d53c2-925d-4f6e-bde1-4023f4188d24" />
  c) use an amazon linux AMI
  <img width="723" height="413" alt="image" src="https://github.com/user-attachments/assets/cb5f6f14-9ca5-4aad-941a-63e4ec6ddd5c" />
  d) Choose the right instance type and key pair
  <img width="938" height="383" alt="image" src="https://github.com/user-attachments/assets/34c52c37-380c-49a4-a88b-679d51d862bf" />
  e) Select a security group that allows http and https connections on port 80 and 443 respectively
  <img width="946" height="379" alt="image" src="https://github.com/user-attachments/assets/a08b5599-0905-4997-b62f-378b6d54eee5" />
  f) Insert a user data and launch
  <img width="685" height="351" alt="image" src="https://github.com/user-attachments/assets/70f20714-1b7f-47a8-8a70-2081aaef907a" />
  <img width="781" height="326" alt="image" src="https://github.com/user-attachments/assets/5eef6477-d43b-4f94-b53d-04e4c371960c" />

2. Set up an auto scaling group
   a) navigate to auto scaling group on the left panel
   <img width="682" height="389" alt="image" src="https://github.com/user-attachments/assets/0a286889-57ec-40a7-8846-6d50d5ef7e6e" />
   b) Select the template as well as provide an auto scaling group name
   <img width="950" height="384" alt="image" src="https://github.com/user-attachments/assets/229f3946-7022-4fd9-9c5f-ee597c134da7" />
   <img width="917" height="376" alt="image" src="https://github.com/user-attachments/assets/8cce30b0-7657-4e70-91cb-1d60241e8f45" />
   c) Configure the network. Provison another public subnet
   <img width="937" height="376" alt="image" src="https://github.com/user-attachments/assets/8aea891f-2253-4dbc-8833-dd10fc94b694" />
   <img width="928" height="380" alt="image" src="https://github.com/user-attachments/assets/147d0f01-7f24-4bbd-b7a0-2935f6675b6a" />
   <img width="942" height="385" alt="image" src="https://github.com/user-attachments/assets/60afa3cd-2654-4b3f-98ec-708d9f2fb2c5" />
   d) Integrate load balancer, health checks, target groups and other services
   <img width="945" height="376" alt="image" src="https://github.com/user-attachments/assets/cc05484d-e3f5-437c-96ec-0851110bcbe8" />
   <img width="943" height="377" alt="image" src="https://github.com/user-attachments/assets/c6ed69ce-e75f-4dda-96b9-ff633e974f09" />
   <img width="940" height="386" alt="image" src="https://github.com/user-attachments/assets/2421158b-ca18-467e-b094-f2200da29066" />
   <img width="942" height="380" alt="image" src="https://github.com/user-attachments/assets/d775ba7c-5c2e-4233-ad97-9aaf1e4598dd" />
   <img width="940" height="386" alt="image" src="https://github.com/user-attachments/assets/546cd23a-9c3b-4886-9d2a-9c5d8b1483e2" />
   e) Configure group size and scaling policy
   <img width="956" height="390" alt="image" src="https://github.com/user-attachments/assets/2a1e8812-2a5a-4cc5-a7b8-a65efc66e8b7" />
   <img width="946" height="378" alt="image" src="https://github.com/user-attachments/assets/291eba35-aed3-486f-ac2d-591e98caa1ba" />
   <img width="937" height="382" alt="image" src="https://github.com/user-attachments/assets/a4cf95f3-bba1-449d-aebb-7beb33df7b6e" />
   <img width="947" height="390" alt="image" src="https://github.com/user-attachments/assets/f8ea0671-b8cd-4da9-9ce1-a652e1567a2c" />
   f) Craete the autoscaling group
   <img width="941" height="383" alt="image" src="https://github.com/user-attachments/assets/f51f4f42-ec49-43e8-8293-72aa439705a3" />


3. Instance display
   a) running minimum number of instances
   <img width="956" height="424" alt="image" src="https://github.com/user-attachments/assets/ae83c39d-4bca-4cc9-b167-fadbab826300" />
   b) auto scaliing setup
   <img width="957" height="386" alt="image" src="https://github.com/user-attachments/assets/acca67da-88fd-49da-9674-73f3284baf27" />
4. Generate stress for instances
   a) Heatlhy instances check
   <img width="953" height="393" alt="image" src="https://github.com/user-attachments/assets/1775a596-e993-43b8-8eee-08e294065215" />
   <img width="736" height="154" alt="image" src="https://github.com/user-attachments/assets/19655a40-095e-4e8e-bf22-5bfeac86da14" />
   <img width="959" height="222" alt="image" src="https://github.com/user-attachments/assets/453dd21a-3919-430f-836b-b214b6d604cd" />






















