# Ansible Playbook Explanation

This playbook is used to set up a Raspberry Pi by installing necessary packages, configuring SSH, creating a swap file, and setting up Docker and Docker Compose.

## Playbook Structure

### Hosts Definition

```yaml
- hosts: raspberry_pi
  become: true
```
# Ansible Playbook Explanation

This playbook is used to set up a Raspberry Pi by installing necessary packages, configuring SSH, creating a swap file, and setting up Docker and Docker Compose.

## Playbook Structure

### Hosts Definition

```yaml
- hosts: raspberry_pi
  become: true
```

- `hosts: raspberry_pi`: Specifies the target host group (in this case, Raspberry Pi). The actual IP or hostname of the Raspberry Pi would be defined in the Ansible inventory file.
- `become: true`: Indicates that tasks will be run with elevated privileges (as the root user), typically using `sudo`.

### Tasks

#### 1. Update and Upgrade System

```yaml
- name: Update package list and upgrade system
  apt:
    update_cache: yes
    upgrade: dist
    cache_valid_time: 3600
```

- `name`: A description of the task, "Update package list and upgrade system".
- `apt`: This Ansible module is used to manage packages on Debian/Ubuntu systems.
  - `update_cache: yes`: Refresh the package cache before running the task.
  - `upgrade: dist`: Perform a full system upgrade, including kernel and major package upgrades.
  - `cache_valid_time: 3600`: Cache validity period, in this case, 1 hour (3600 seconds).

#### 2. Install Necessary Dependencies

```yaml
- name: Install necessary dependencies
  apt:
    name:
      - apt-transport-https
      - ca-certificates
      - curl
      - software-properties-common
    state: present
```

- `name`: Description of the task, "Install necessary dependencies".
- `apt`: Again using the apt module to install packages.
  - `name`: List of packages to install: `apt-transport-https`, `ca-certificates`, `curl`, and `software-properties-common`.
  - `state: present`: Ensures the packages are installed.

#### 3. Create Swap File

```yaml
- name: Create swap file
  command: fallocate -l 1G /swapfile
```

- `name`: "Create swap file".
- `command`: Runs the Linux command `fallocate` to create a 1GB swap file at `/swapfile`.

#### 4. Set Permissions for Swap File

```yaml
- name: Set correct permissions on the swap file
  file:
    path: /swapfile
    mode: '0600'
```

- `name`: "Set correct permissions on the swap file".
- `file`: Ansible module for managing file attributes.
  - `path`: Path to the swap file.
  - `mode: '0600'`: Sets the file permissions so only the root user has read/write access.

#### 5. Initialize Swap File

```yaml
- name: Initialize the swap file
  command: mkswap /swapfile
```

- `name`: "Initialize the swap file".
- `command`: Runs the Linux command `mkswap` to initialize the swap file.

#### 6. Activate Swap File

```yaml
- name: Activate the swap file
  command: swapon /swapfile
```

- `name`: "Activate the swap file".
- `command`: Runs the Linux command `swapon` to activate the swap file.

#### 7. Change SSH Port to 2222

```yaml
- name: Change SSH port to 2222
  lineinfile:
    path: /etc/ssh/sshd_config
    regexp: '^#?Port'
    line: 'Port 2222'
    state: present
```

- `name`: "Change SSH port to 2222".
- `lineinfile`: Ansible module to manage lines in a file.
  - `path`: Path to the SSH configuration file.
  - `regexp`: Regular expression to match the current port line.
  - `line`: Replaces the port line with `Port 2222`.
  - `state: present`: Ensures the line is present.

#### 8. Disable Password Authentication

```yaml
- name: Disable password authentication
  lineinfile:
    path: /etc/ssh/sshd_config
    regexp: '^#?PasswordAuthentication'
    line: 'PasswordAuthentication no'
    state: present
```

- `name`: "Disable password authentication".
- `lineinfile`: Modifies `/etc/ssh/sshd_config` to disable password-based authentication.

#### 9. Restart SSH Service

```yaml
- name: Restart SSH service
  service:
    name: ssh
    state: restarted
```

- `name`: "Restart SSH service".
- `service`: Ansible module to manage services.
  - `name`: Service name (`ssh`).
  - `state: restarted`: Ensures the SSH service is restarted to apply configuration changes.

#### 10. Install Docker

```yaml
- name: Install Docker
  apt:
    name: docker.io
    state: present
```

- `name`: "Install Docker".
- `apt`: Installs the Docker package (`docker.io`).

#### 11. Start Docker Service

```yaml
- name: Start Docker service
  systemd:
    name: docker
    enabled: yes
    state: started
```

- `name`: "Start Docker service".
- `systemd`: Manages system services.
  - `name: docker`: Service name.
  - `enabled: yes`: Ensures Docker starts on boot.
  - `state: started`: Starts the Docker service.

#### 12. Install Docker Compose

```yaml
- name: Download Docker Compose
  get_url:
    url: https://github.com/docker/compose/releases/download/1.29.2/docker-compose-Linux-armv7l
    dest: /usr/local/bin/docker-compose
    mode: '0755'
```

- `name`: "Download Docker Compose".
- `get_url`: Downloads the Docker Compose binary.
  - `url`: URL of the Docker Compose binary for ARM architecture.
  - `dest`: Destination path for the binary.
  - `mode: '0755'`: Sets executable permissions.

#### 13. Verify Docker Compose Installation

```yaml
- name: Verify Docker Compose installation
  command: docker-compose --version
```

- `name`: "Verify Docker Compose installation".
- `command`: Runs the `docker-compose --version` command to verify the installation.



- `hosts: raspberry_pi`: Specifies the target host group (in this case, Raspberry Pi). The actual IP or hostname of the Raspberry Pi would be defined in the Ansible inventory file.
- `become: true`: Indicates that tasks will be run with elevated privileges (as the root user), typically using `sudo`.

### Tasks

#### 1. Update and Upgrade System

```yaml
- name: Oppdater pakkelisten og oppgrader systemet
  apt:
    update_cache: yes
    upgrade: dist
    cache_valid_time: 3600
```

- `name`: A description of the task, "Oppdater pakkelisten og oppgrader systemet" means "Update package list and upgrade system".
- `apt`: This Ansible module is used to manage packages on Debian/Ubuntu systems.
  - `update_cache: yes`: Refresh the package cache before running the task.
  - `upgrade: dist`: Perform a full system upgrade, including kernel and major package upgrades.
  - `cache_valid_time: 3600`: Cache validity period, in this case, 1 hour (3600 seconds).

#### 2. Install Necessary Dependencies

```yaml
- name: Installer nødvendige avhengigheter
  apt:
    name:
      - apt-transport-https
      - ca-certificates
      - curl
      - software-properties-common
    state: present
```

- `name`: Description of the task, "Installer nødvendige avhengigheter" means "Install necessary dependencies".
- `apt`: Again using the apt module to install packages.
  - `name`: List of packages to install: `apt-transport-https`, `ca-certificates`, `curl`, and `software-properties-common`.
  - `state: present`: Ensures the packages are installed.

#### 3. Create Swap File

```yaml
- name: Opprett swap-fil
  command: fallocate -l 1G /swapfile
```

- `name`: "Opprett swap-fil" means "Create swap file".
- `command`: Runs the Linux command `fallocate` to create a 1GB swap file at `/swapfile`.

#### 4. Set Permissions for Swap File

```yaml
- name: Sett riktig tillatelser på swap-filen
  file:
    path: /swapfile
    mode: '0600'
```

- `name`: "Sett riktig tillatelser på swap-filen" means "Set correct permissions on the swap file".
- `file`: Ansible module for managing file attributes.
  - `path`: Path to the swap file.
  - `mode: '0600'`: Sets the file permissions so only the root user has read/write access.

#### 5. Initialize Swap File

```yaml
- name: Init swap-filen
  command: mkswap /swapfile
```

- `name`: "Init swap-filen" means "Initialize the swap file".
- `command`: Runs the Linux command `mkswap` to initialize the swap file.

#### 6. Activate Swap File

```yaml
- name: Aktiver swap-filen
  command: swapon /swapfile
```

- `name`: "Aktiver swap-filen" means "Activate the swap file".
- `command`: Runs the Linux command `swapon` to activate the swap file.

#### 7. Change SSH Port to 2222

```yaml
- name: Endre SSH-port til 2222
  lineinfile:
    path: /etc/ssh/sshd_config
    regexp: '^#?Port'
    line: 'Port 2222'
    state: present
```

- `name`: "Endre SSH-port til 2222" means "Change SSH port to 2222".
- `lineinfile`: Ansible module to manage lines in a file.
  - `path`: Path to the SSH configuration file.
  - `regexp`: Regular expression to match the current port line.
  - `line`: Replaces the port line with `Port 2222`.
  - `state: present`: Ensures the line is present.

#### 8. Disable Password Authentication

```yaml
- name: Deaktiver passordinnlogging
  lineinfile:
    path: /etc/ssh/sshd_config
    regexp: '^#?PasswordAuthentication'
    line: 'PasswordAuthentication no'
    state: present
```

- `name`: "Deaktiver passordinnlogging" means "Disable password authentication".
- `lineinfile`: Modifies `/etc/ssh/sshd_config` to disable password-based authentication.

#### 9. Restart SSH Service

```yaml
- name: Start SSH på nytt
  service:
    name: ssh
    state: restarted
```

- `name`: "Start SSH på nytt" means "Restart SSH".
- `service`: Ansible module to manage services.
  - `name`: Service name (`ssh`).
  - `state: restarted`: Ensures the SSH service is restarted to apply configuration changes.

#### 10. Install Docker

```yaml
- name: Installer Docker
  apt:
    name: docker.io
    state: present
```

- `name`: "Installer Docker" means "Install Docker".
- `apt`: Installs the Docker package (`docker.io`).

#### 11. Start Docker Service

```yaml
- name: Start Docker-tjenesten
  systemd:
    name: docker
    enabled: yes
    state: started
```

- `name`: "Start Docker-tjenesten" means "Start Docker service".
- `systemd`: Manages system services.
  - `name: docker`: Service name.
  - `enabled: yes`: Ensures Docker starts on boot.
  - `state: started`: Starts the Docker service.

#### 12. Install Docker Compose

```yaml
- name: Last ned Docker Compose
  get_url:
    url: https://github.com/docker/compose/releases/download/1.29.2/docker-compose-Linux-armv7l
    dest: /usr/local/bin/docker-compose
    mode: '0755'
```

- `name`: "Last ned Docker Compose" means "Download Docker Compose".
- `get_url`: Downloads the Docker Compose binary.
  - `url`: URL of the Docker Compose binary for ARM architecture.
  - `dest`: Destination path for the binary.
  - `mode: '0755'`: Sets executable permissions.

#### 13. Verify Docker Compose Installation

```yaml
- name: Sjekk Docker Compose-versjonen
  command: docker-compose --version
```

- `name`: "Sjekk Docker Compose-versjonen" means "Check Docker Compose version".
- `command`: Runs the `docker-compose --version` command to verify the installation.