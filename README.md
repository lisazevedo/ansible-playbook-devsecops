
# k3s Installation

This project provides an Ansible role to install, configure, and clean up k3s on a Linux host. It automates the installation process, verifies service status, and cleans up dependencies and installations when needed.

## Requirements

- Ansible >= 2.9.
    Ensure Ansible is installed on your control machine.

- Python >= 3.6 

- SSH Access to the target hosts with a valid SSH key configured.

4. **Target Hosts**:
   - A Linux-based system where k3s will be installed.
   - Ensure that `sudo` access is available on the target hosts for privilege escalation.

## Dependencies:
Required packages: 
`apt-transport-https`, `curl`, `software-properties-common`, `gnupg`.

These packages are installed automatically by the playbook, but ensure your target system can access the necessary repositories.

## Directory Structure

- `roles/k3s/tasks/main.yaml`: Main task file that includes the `install` and `cleanup` tasks.
- `roles/k3s/tasks/install.yaml`: Contains tasks for installing k3s, including dependencies, running the installation script, and verifying the service.
- `roles/k3s/tasks/cleanup.yaml`: Contains tasks for cleaning up after the k3s installation, including removing dependencies and uninstalling k3s.
- `hosts`: Inventory file specifying host details.
- `playbook.yaml`: Playbook file that includes the `k3s` role.
- `ansible.cfg`: Ansible configuration file.

## Playbook Breakdown

### Roles

- **k3s**: Includes tasks for installing and cleaning up k3s.

### Tasks

- **Install Dependencies:**
  Installs required packages, including `gnupg` for GPG verification if needed.

- **Download k3s Install Script:**
  Downloads the k3s installation script from the official source.

- **Run k3s Install Script:**
  Executes the k3s installation script.

- **Verify Service Status:**
  Checks if the k3s service is active and running.

- **Remove Dependencies:**
  Cleans up dependencies used during installation.

- **Check and Remove k3s:**
  Checks if the uninstall script exists and removes k3s if present.

## Installation and Usage

1. **Clone the Repository:**

   ```bash
   git clone https://github.com/lisazevedo/ansible-playbook-devsecops.git
   cd ansible-playbook-devsecops
   ```

2. **Update Inventory File:**

   Edit the `hosts` file to specify your target hosts and SSH key path.

3. **Run the Playbook:**

   You can run the entire playbook with the following command:

   ```bash
   ansible-playbook playbook.yaml
   ```

   This command executes all tasks defined in the playbook. However, if you want to run specific parts of the playbook, you can use tags to control which tasks are executed.

### Tags

Tags allow you to selectively run tasks based on their tags. In this playbook, there are two main tags defined: `install` and `cleanup`.

- **Install Tasks:**

  Tasks tagged with `install` are related to the installation of k3s and its dependencies. To run only these tasks, use the following command:

  ```bash
  ansible-playbook playbook.yaml --tags=install
  ```

  This command will execute only the tasks tagged with `install` and skip all others. This is useful when you want to perform or re-run only the installation tasks without affecting the cleanup.

- **Cleanup Tasks:**

  Tasks tagged with `cleanup` handle the removal of k3s and its dependencies. To run only these tasks, use the following command:

  ```bash
  ansible-playbook playbook.yaml --tags=cleanup
  ```

  This command will execute only the tasks tagged with `cleanup`. It’s useful for cleaning up resources without reinstalling k3s.

## What Can Be Improved

**GPG Verification:**
   Currently, GPG verification is commented out due to issues with finding the public key and signature files. Implementing GPG verification will enhance security by ensuring the authenticity of the installation script. Update URLs for public keys and signatures as necessary.


Here’s a more detailed explanation about using tags in your Ansible playbook and how to run the playbook with specific tags.
