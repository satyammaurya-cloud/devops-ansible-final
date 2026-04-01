#  Ansible Dynamic Inventory on AWS

This guide outlines the process for setting up a Dynamic Inventory in Ansible using the `aws_ec2` plugin to automatically discover and manage EC2 instances.

## Phase 1: Environment Setup
- **Launch Ansible Master:** Provision an EC2 instance (Amazon Linux 2/2023 recommended).
- **Attach IAM Role:** Create an IAM Role with `AmazonEC2FullAccess` (or a policy allowing `ec2:Describe*`) and attach it to the `Ansible-Master`.
- **Launch Managed Node:** Create a second EC2 instance named `Name:dev`.
- **Establish SSH Trust:**
  - On Master: Run `ssh-keygen`.
  - On Master: Copy the content of `~/.ssh/id_rsa.pub`.
  - On Managed Node: Paste that key into `~/.ssh/authorized_keys`.

## Phase 2: Install Dependencies (Master Node)
Run these commands to prepare the environment:
```bash
sudo yum update -y
sudo yum install ansible -y
sudo yum install python3-pip -y
pip3 install boto3
ansible-galaxy collection install amazon.aws
```
*Use code with caution.*

## Phase 3: Configure Ansible
1. **Edit `ansible.cfg`:**
   - Update the configuration to enable the plugin:
   ```bash
   sudo vi /etc/ansible/ansible.cfg
   ```
   
   - Add or update these lines:
    ```bash
    [defaults]
    host_key_checking = False
    enable_plugins = amazon.aws.aws_ec2
    inventory = ./aws_ec2.yml
    ```
     
2. **Create Dynamic Inventory File (`aws_ec2.yml`)**
   - Create and edit the file:
    ```bash
    sudo vi /etc/ansible/aws_ec2.yml
    ```
   - Paste this basic filter configuration:
    ```yaml
    plugin: amazon.aws.aws_ec2
    regions:
        - us-east-1
    filters:
        tag:Name:
            - dev
    ```

## Phase 4: Verification & Execution
1. **Test Connectivity**
```bash
ansible-inventory -i aws_ec2.yml --graph
ansible all -m ping
```
*Use code with caution.*

2. **Deploy Playbook**
Create `deploy.yml` to install a web server:
```yaml
- name: install webserver
  hosts: all
  become: yes
  tasks:
    - yum:
        name: httpd
        state: latest
    - service:
        name: httpd
        state: started
```


3. **Run the Playbook**
```bash
ansible-playbook deploy.yml
```

## Phase 5: Advanced Grouping (Optional)
To automatically group instances based on tags, update your `aws_ec2.yml`:
```yaml
plugin: amazon.aws.aws_ec2
regions:
  - us-east-1
keyed_groups:
  - key: tags.Name
    prefix: tag_Name_
    separator: ""
groups:
  development: "'dev' in tags.Name"
filters:
  tag:Name:
    - dev
```

- **Result:** This creates a group called tag_Name_dev and a custom group called development.
- **Verify:** Run ansible-inventory --list to see the new group structure.
---
### Summary of Results

- **Dynamic Discovery:** Instances are automatically added to the inventory based on the `dev` tag.
- **Automation:** Web server deployment is handled across all discovered nodes simultaneously.
- **Scalability:** As you launch more "dev" instances, Ansible will pick them up without manual configuration.
---
