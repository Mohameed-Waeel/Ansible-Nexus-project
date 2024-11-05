# Ansible-Nexus-project

This Ansible playbook automates the process of downloading, installing, configuring, and starting Sonatype Nexus on a target server. It includes tasks for installing dependencies, downloading the Nexus installer, creating required directories, managing user permissions, and verifying that Nexus is running.

## Prerequisites

- **Ansible**: Ensure Ansible is installed on the control node.
- **Target System**: The target server should be accessible via SSH and configured in the Ansible inventory.
- **Root Privileges**: Ensure that the playbook has privileges to install packages and manage system services on the target server.
- **Java Installation**: Nexus requires Java to run, which this playbook installs if not already available.

## Playbook Structure

The playbook is organized into the following plays:

1. **Install Java and Net-Tools**: Installs required dependencies such as Java 8 and Net-Tools.
2. **Download and Unpack Nexus**: Downloads the latest Nexus installer, unpacks it, and sets up the required directory structure.
3. **Create Nexus User**: Configures a dedicated user and group to run Nexus securely.
4. **Start Nexus as Nexus User**: Configures Nexus to run as the `nexus` user and starts the service.
5. **Verify Nexus is Running**: Checks if Nexus is running and accessible on the specified port.

## Usage

1. **Clone the Repository**: Clone or download this playbook to your local machine.

   ```bash
   git clone <repository_url>
   ```

2. **Configure Inventory**: Specify the target host in your Ansible inventory file (e.g., `/etc/ansible/hosts`).

3. **Run the Playbook**: Execute the playbook by running the following command:

   ```bash
   ansible-playbook nexus_playbook.yml
   ```

4. **Verify Installation**: The playbook will print debug messages to confirm Nexus is running, showing its process ID and network port.

## Playbook Tasks

### Install Java and Net-Tools

- Updates the DNF cache and installs Java  and `net-tools` (for checking network status).
  
### Download and Unpack Nexus

- **Download Nexus**: Downloads the latest Nexus installer to `/opt`.
- **Unpack Nexus**: Extracts the installer in `/opt/`, creating a folder named `nexus-*`.
- **Rename Folder**: Renames the extracted folder to `/opt/nexus` if it doesn’t already exist.

### Create Nexus User

- **User and Group Creation**: Creates a `nexus` user and group to own the Nexus folders.
- **Ownership Assignment**: Sets ownership of `/opt/nexus` and `/opt/sonatype-work` to the `nexus` user and group.

### Start Nexus as Nexus User

- **Set Run User**: Configures Nexus to run under the `nexus` user by updating `nexus.rc`.
- **Start Nexus**: Starts the Nexus service.

### Verify Nexus is Running

- **Process Verification**: Confirms the Nexus process is running by using the `ps` command.
- **Network Verification**: Uses `netstat` to check that Nexus is listening on the appropriate port.

## Expected Output

After running the playbook, you should see debug messages confirming:

- Nexus installation status.
- Nexus process running status (via `ps`).
- Nexus port status (via `netstat`).

## Notes

- **Configuration Files**: The `nexus.rc` file is updated to ensure Nexus runs under the correct user (`nexus`).
- **Ports**: Nexus typically runs on port `8081` by default. Ensure this port is open and accessible.

## Troubleshooting

- **Java Version**: Nexus may not support versions of Java higher than 8. Adjust the playbook if you need a specific version.
- **Permissions**: Verify that the playbook has the necessary permissions for all tasks, especially for modifying directories and files.
- **Network Access**: Ensure that firewalls or security groups allow access to the Nexus port.

---
