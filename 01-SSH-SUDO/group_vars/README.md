
# Ansible Playbook: Set Up SSH Key-Based Authentication and Grant Passwordless Sudo

## Overview

This Ansible playbook automates the setup of SSH key-based authentication for a specified user on remote servers and grants passwordless sudo privileges to that user. The playbook ensures that the `.ssh` directory and `authorized_keys` file are properly configured on each remote server and that the public key from the Ansible controller is copied to the remote servers. Additionally, it modifies the `sudoers` file to allow passwordless sudo for the specified user.

## Prerequisites

1. **Ansible Installed**: Ensure that Ansible is installed on the controller machine.
2. **SSH Access**: The user running the playbook must have SSH access to the remote servers.
3. **Public Key**: The public key (`id_rsa.pub`) must be generated on the Ansible controller machine.

## Inventory Configuration

The playbook is designed to run on a group of hosts named `cluster`. Ensure that your Ansible inventory file (`hosts`) is configured with a group named `cluster` containing the target servers.

Example `hosts` file:

\`\`\`ini
[cluster]
server1.example.com
server2.example.com
\`\`\`

## Variables

### `group_vars/cluster/vars.yml`

This file contains non-sensitive variables used by the playbook. Customize it according to your environment.

Example:

\`\`\`yaml
ansible_user: your_username
\`\`\`

### `group_vars/cluster/vault.yml`

This file contains sensitive variables, such as passwords or private keys. It is encrypted using Ansible Vault to protect sensitive data.

Example:

\`\`\`yaml
# Encrypted using Ansible Vault
ansible_user_password: "your_encrypted_password"
\`\`\`

To edit the `vault.yml` file, use the following command:

\`\`\`bash
ansible-vault edit group_vars/cluster/vault.yml
\`\`\`

## Playbook Details

### Tasks Overview

1. **Ensure the .ssh directory exists**: Creates the `.ssh` directory on the remote servers if it does not already exist, with appropriate permissions.
2. **Create authorized_keys file**: Ensures that the `authorized_keys` file exists in the `.ssh` directory.
3. **Copy the public key**: Copies the public key from the Ansible controller to the `authorized_keys` file on the remote servers.
4. **Grant passwordless sudo**: Modifies the `sudoers` file to grant the specified user passwordless sudo privileges.

### Usage

1. **Clone the Repository**

   \`\`\`bash
   git clone https://github.com/yourusername/your-repo-name.git
   cd your-repo-name
   \`\`\`

2. **Encrypt Sensitive Variables**

   Before running the playbook, make sure your sensitive variables are encrypted. If they are not, use the following command:

   \`\`\`bash
   ansible-vault encrypt group_vars/cluster/vault.yml
   \`\`\`

3. **Run the Playbook**

   To execute the playbook, run the following command:

   \`\`\`bash
   ansible-playbook setup-ssh-and-sudo.yml --ask-vault-pass
   \`\`\`

   - **\`--ask-vault-pass\`**: Prompts for the Ansible Vault password to decrypt the `vault.yml` file.

4. **Verify the Setup**

   After running the playbook, verify that:
   - The `.ssh` directory and `authorized_keys` file have been created on the remote servers.
   - The public key has been copied to the `authorized_keys` file.
   - The user has passwordless sudo privileges on the remote servers.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contributions

Contributions are welcome! Please open an issue or submit a pull request with any improvements or suggestions.
