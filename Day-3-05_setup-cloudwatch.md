# EC2 Apache and CloudWatch Agent Setup with Ansible

This Ansible playbook automates Amazon CloudWatch Agent setup on EC2. It installs the agent, configures it to collect all logs from `/var/log/*`, and streams them to the `LOG-FROM-EC2` CloudWatch Log Group.

### 🛠️ Prerequisites

Before running the playbook, ensure you have completed these steps:

1.  **IAM Role:** Attach an IAM Role to your target EC2 instance with the **`CloudWatchAgentServerPolicy`** permissions.
2.  **Ansible Installed:** Ensure Ansible is installed on your control node.
3.  **Inventory File:** Create a `inventory` file with your target EC2 IP address.
   
### 🚀 Lab Overview

This setup performs the following actions:
1.  Installs the `amazon-cloudwatch-agent` package.
2.  Creates a custom configuration file (`config.json`) to track logs.
3.  Starts the CloudWatch Agent using the `amazon-cloudwatch-agent-ctl` utility.
4.  Verifies the running status of the agent.

### 📂 Configuration Details

The agent is configured with the following parameters:
*   **Log Group Name:** `LOG-FROM-EC2`
*   **Log Stream Name:** `{instance_id}`
*   **Retention:** 1 Day (to optimize costs)
*   **Log Files:** All files under `/var/log/`

### 📋 How to Use

1.  **Clone this repository** or copy the `playbook.yml` content.
2.  **Update your inventory** file:
    ```ini
    [ec2_instances]
    your-instance-ip ansible_user=ec2-user ansible_ssh_private_key_file=~/your-key.pem
    ```
3.  **Run the playbook:**
    ```bash
    ansible-playbook -i hosts playbook.yml
    ```

### ✅ Verification

Once the playbook finishes successfully:
1.  Log in to the **AWS Management Console**.
2.  Navigate to **CloudWatch** > **Logs** > **Log groups**.
3.  Look for the group named **`LOG-FROM-EC2`**.
4.  You should see log streams identified by your EC2 Instance ID.

---
*Generated for CloudWatch Monitoring Lab*
