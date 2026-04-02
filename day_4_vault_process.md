### Ansible Vault ?
- Used to encrypt sensitive data in YAML files
- Protects passwords, keys, and secrets

1. **Create Encrypted File**

`ansible-vault create secret.yaml`
  > need to provide password

      a. then go to vi editor and place any yaml file and run 
      b. after that try to edit or read the yaml but we are unable to read due to encryption applies

2. **If we want to modify the encrypted yaml file give below command**

`ansible-vault edit secret.yaml`
> and enter given password 
  
3. **If we want to decrypt the yaml file**

`ansible-vault decrypt secret.yaml`
> and give password

4. **If want to encrypt any existing yaml file**

`ansible-vault encrypt secret.yaml`
> provide password to secure 

---

#### Key Points 
- Used for securing sensitive data 
- Password-based encryption 
- Common in production environments
