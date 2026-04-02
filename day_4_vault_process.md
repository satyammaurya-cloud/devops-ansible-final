`ansible-vault create secret.yaml`

> need to provide password
- then go to vi editor and place any yaml file and run 

- after that try to edit or read the yaml but we are unable to read due to encryption applies

- if we want to modify the yaml give below command 

`ansible-vault edit secret.yaml`
> and enter given password 
  
- if we want want decrypt the password
`ansible-vault decrypt secret.yaml`
> and give password

- if want to encrypt the password 
`ansible-vault encrypt secret.yaml`
> to provide secure 

---
## Ansible Vault –  Notes

### 🔹 Ansible Vault ?
- Used to encrypt sensitive data in YAML files
- Protects passwords, keys, and secrets

1. **Create Encrypted File**
```bash
ansible-vault create secret.yaml
```
    - Prompts for a password
    - Creates an encrypted YAML file

2. **Behavior After Encryption**
  - File content cannot be read directly
  - Even if opened in editor, data is encrypted

3. **Edit Encrypted File**
```bash
ansible-vault edit secret.yaml
```
    - Requires vault password
    - Opens file securely for editing

4. **Decrypt File**
```bash
ansible-vault decrypt secret.yaml
```
- Converts encrypted file to plain text
- Requires password

5. **Encrypt Existing File**
```bash
ansible-vault encrypt secret.yaml
```
- Encrypts an already existing YAML file 
 
## 🔥 Key Points 
- Used for securing sensitive data 
- Password-based encryption 
- Common in production environments
