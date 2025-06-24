# Ansible User Management Playbook

## This playbook creates new users and groups and configures SSH keys for them. It also optionally removes users

## Pre-requisites

- Ansible 2.9 or newer
- Python 3.6 or newer

## Usage

1. Install Ansible and Python 3.6 or newer
2. Clone the repository
3. Configure the `inventory.ini` file with your hosts
4. Edit the variables in the `roles/users/vars/main.yml` file to your liking
5. Add in your SSH public keys to the `roles/users/files/keys` directory. Must be named `<username>.pub` for the keys to be applied correctly
6. Optionally remove users by setting the `users_remove` variable to the name of the user you want to remove
7. Run the playbook

## Example

```bash
# Create new users and groups with SSH keys
ansible-playbook -i inventory.yml users-playbook.yml

# Remove the user 'bob'
ansible-playbook -i inventory.yml users-playbook.yml -e users_remove=bob
```

# Ansible Backup and Restore Playbook

## This playbook backs up a specified directory to a timestamped `.tar.gz` archive. It can also optionally roll back the directory to the most recent backup.

## Pre-requisites

- Ansible 2.9 or newer
- Python 3.6 or newer

## Usage

1. Install Ansible and Python 3.6 or newer
2. Clone the repository
3. Configure the `inventory.ini` file with your hosts under the target `[backup_hosts]`
4. Configure the variables in the roles/backup/vars/main.yml file to match your environment.
    * `$source_dir$`: The absolute path to the directory you want to back up.

    * `$backup_dir$`: The absolute path to the directory where backups will be stored. The user running the playbook must have write permissions to this directory.

    * `$rollback$`: Set to `false` (default) to perform a backup, or true to perform a restore. This is typically overridden at runtime.
5. Run the Playbook: Execute the playbook from your terminal. The playbook runs with `become: true`, so it will request sudo privileges on the remote hosts.

## Example

```bash
# Create a backup of the source directory
ansible-playbook -i inventory.yml backup-playbook.yml

# Restore the source directory from the latest backup
ansible-playbook -i inventory.yml backup-playbook.yml -e "rollback=true"
```

# Ansible System Compliance Playbook

## This playbook audits and enforces system compliance by ensuring specific packages and services are in their desired state.

## Pre-requisites

* Ansible 2.9 or newer
* Python 3.6 or newer

## Usage

1. Install Ansible and Python: Ensure Ansible 2.9+ and Python 3.6+ are installed on your control node.

2. Clone the repository: Download the playbook files to your local machine.

3. Configure your inventory: Edit the `inventory.ini` file to include the hosts you want to audit under the `[compliance_hosts]` group.

4. Define compliance state: Edit the variables in the `roles/compliance/vars/main.yml` file to define the desired state for your systems.

    * `required_pkgs:` A list of packages that must be installed.

    * `forbidden_pkgs:` A list of packages that must not be installed.

    * `enabled_services`: A list of services that must be running and enabled on boot.

    * `disabled_services`: A list of services that must be stopped and disabled.

5. Run the playbook: Execute the playbook from your terminal. The playbook runs with `become: true`, so it will request sudo privileges on the remote hosts to manage packages and services.

## Example
```bash
# Run the compliance audit and enforcement on all targeted hosts
ansible-playbook -i inventory.yml compliance-playbook.yml
```
