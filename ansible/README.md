## Ansible automation 

### Using Ansible Vault to securely store secrets
**Ansible Vault** is used to store secrets in an encrypted form across Ansible configurations.  
The secrets are encrypted and decrypted on the fly using a symmetric key stored in an external secret manager (macOS Keychain).

#### Preparation
1. Create a new symmetric key (using `pwgen`) and store it in macOS Keychain (using `security`)
    ```bash
    ACCOUNT_NAME="mjk"
    SERVICE_NAME="ansible-vault"
    /usr/bin/security add-generic-password -a "$ACCOUNT_NAME" -s "$SERVICE_NAME" -D "application password" -w `pwgen -N 1 16`
    ```

2. This script will be used by Ansible to retrieve the symmetric key from macOS Keychain.  
    Save it as `$HOME/.retrieve-vault-key-from-keychain.sh`
    ```bash
    #!/bin/bash
    ACCOUNT_NAME="mjk"
    LABEL="ansible-vault"
    /usr/bin/security find-generic-password -w -a "$ACCOUNT_NAME" -s "$SERVICE_NAME"
    ```

3. Append the following line to your `$HOME/.ansible.cfg` under the `[defaults]` section:
    ```
    vault_password_file = .retrieve-vault-key-from-keychain.sh
    ```

#### Usage
Let us assume you use group variable files. 
```
ansible
└── group_vars
    └── workers
        ├── vars
        └── vault
```
1. Store all configurations in the `ansible/group_vars/workers/vars` files, but use for secrets reference another variables:
    ```
    ---
    someuser_name: userone
    someuser_password: "{{ vault_userone_password }}"
    ``` 
2. Put the aforementioned referenced variables to the `ansible/group_vars/workers/vault` files:
    ```
    ---
    vault_userone_password: SomeFakeSecretPassword
    ``` 
3. Run the following command:
    ```bash
    ansible-vault encrypt ansible/group_vars/workers/vault
    ```
    The entire file will get encrypted using the symmetric key stored in macOS Keychain.

4. From now on, you will be able to view and edit the encrypted `vault` files through the following commands:
    ```bash
    ansible-vault view ansible/group_vars/workers/vault
    ansible-vault edit ansible/group_vars/workers/vault
    ```
