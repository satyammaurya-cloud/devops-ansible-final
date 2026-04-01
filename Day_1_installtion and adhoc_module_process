# Ansible Guide

---

### Installing Ansible

```bash
yum install ansible -y
ansible --version
```

#### Setting Up SSH Key for Authentication
- Generate SSH key and copy the public IP:
  ```bash
  ssh-keygen 
  ```
>copy public ip and paste it into target server path .ssh/authorized_keys 
---

#### Sample Ad-Hoc Commands in Ansible
*Ad-hoc commands are commands which can be run individually to perform quick functions.*
  ```bash
  ansible -i inventory all -m shell -a "touch file100"
  ```

#### Inventory Basics
- An inventory is a list of managed nodes or hosts that Ansible deploys and configures.
- This guide covers:
  - Creating inventories to track servers and devices for automation.
- Default inventory path: `/etc/ansible/hosts`
- If the target node is added to the default path, no need to specify `-i inventory` in commands; it fetches automatically.
- To edit inventory:
  ```bash
  vi inventory 
  ```
- Add target private IPs.
- Example command:
  ```bash
  ansible -i inventory all -m shell -a "touch file1"
  ```

---

### Common Ansible Modules and Usage Examples
##### Ping Module
```bash
ansible -i inventory all -m ping 
```
##### Stat Module (to check file/directory status)
```bash
ansible -i inventory all -m stat -a "path=/var/www/html"
```
##### Yum or Apt Module (for package management)
```bash
ansible -i inventory all -m yum -a "name=httpd state=present"    
```
##### User Module (manage users)
```bash
gansible -i inventory all -m user -a "name=naresh" -b 
```
##### Setup Module (gather system facts)
```bash
gansible -i inventory all -m setup 
```
##### File Module (manage files/directories)
```bash
gansible -i inventory all -m file -a "name=demo state=touch"
```
##### Package States

```yaml
(name = httpd install
        state=latest
        state=present
        state=absent )
```
> Commands:
```yaml
ansible all -m yum -a "name=httpd state=latest" -b
ansible all -m yum -a "name=httpd state=present" -b
ansible all -m yum -a "name=httpd state=absent" -b
```

##### Service States

```plaintext
(name = systemctl or service              
               state=stopped
               state=started
               state=restarted)
```
> Commands:
```yaml
ansible all -m service -a "name=httpd state=started" -b        #to start service
ansible all -m service -a "name=httpd state=stopped" -b        #to stop service 
ansible all -m service -a "name=httpd state=restrted" -b       #to stop service 
```
