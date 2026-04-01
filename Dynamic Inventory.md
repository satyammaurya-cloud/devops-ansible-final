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
    inventory = ./aws_ec2.yml
    
    [inventory]
    enable_plugins = amazon.aws.aws_ec2
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
> Go to ansible directory and apply below command `cd /etc/ansible`
```bash
ansible-inventory -i aws_ec2.yml --graph
ansible all -m ping
```

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
  development: "dev” in (tags|list)"
filters:
  tag:Name:
    - dev
```
> Test with group
```bash
ansible tag_Name_dev -m ping
```

- **Result:** This creates a group called `tag_Name_dev` and a custom group called development.
- **Verify:** Run `ansible-inventory --list` to see the new group structure.
---
### Summary of Results

- **Dynamic Discovery:** Instances are automatically added to the inventory based on the `dev` tag.
- **Automation:** Web server deployment is handled across all discovered nodes simultaneously.
- **Scalability:** As you launch more "dev" instances, Ansible will pick them up without manual configuration.
---
## Tools and Their Functions

1. **boto3 (The SDK)**
```yaml   
- AWS SDK for python used to interact with AWS service.
- When the Ansible plugin wants to find your servers, it tells boto3 to make the actual API calls to AWS to
  fetch real-time data like IP addresses and their tags. 
- Example: It retrieves the Private IP of your instance.

pip3 install boto3

```
2. **ansible-galaxy**
```yaml   
- A command-line tool used to install community`s pre-built automation "collections" and "roles"
- Example: Similar to an App Store for Ansible.

ansible-galaxy
```   
3. **amazon.aws (The Plugin)**
```yaml   
- Provides the aws_ec2 inventory plugin. This plugin reads a configuration file (e.g., inventory.aws_ec2.yml)
  and determines which AWS regions or tags to scan.
- Automatically detect and group your active AWS servers so you don't have to maintain a manual list of IP addresses.
- Example: It allows you to use `plugin: amazon.aws.aws_ec2` in your config.

ansible-galaxy collection install amazon.aws
```
4. **Dynamic Discovery**
```yaml   
- Together, these tools automatically find your servers so you don't have to type IP addresses manually.
- Example: Finding all instances with the tag `Name: dev`.
```

---
