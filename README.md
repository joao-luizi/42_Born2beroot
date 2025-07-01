# Born2beroot

## Summary
A system administration project from the 42 curriculum. The goal is to configure a secure and minimal Debian server inside a virtual machine, while applying rigorous password, user, firewall, and logging policies. Bonus tasks include hosting a WordPress site and setting up additional services.

---

## Table of Contents

- [Mandatory Part](#mandatory-part)
  - [Operating System](#operating-system)
  - [Partitioning with LVM](#partitioning-with-lvm)
  - [User and Group Management](#user-and-group-management)
  - [Password Policy](#password-policy)
  - [Sudo Configuration](#sudo-configuration)
  - [Firewall Configuration](#firewall-configuration)
  - [SSH Configuration](#ssh-configuration)
  - [Monitoring Script](#monitoring-script)
- [Bonus Part](#bonus-part)
  - [WordPress (LLMP Stack)](#wordpress-llmp-stack)
  - [Additional Services](#additional-services)
- [Disk Signature](#disk-signature)
- [License](#license)

---

## Mandatory Part

### Operating System
- **OS**: Debian
- **Shell only**: No graphical interface, as per subject.
- **Virtualization**: VirtualBox (no snapshots used)

---

### Partitioning with LVM

| Mount Point | Type  | Encrypted | LVM |
|-------------|-------|-----------|-----|
| `/boot`     | ext4  | ❌         | ❌   |
| `/`         | ext4  | ✅         | ✅   |
| `/home`     | ext4  | ✅         | ✅   |

---

### User and Group Management

- Created user: `<login>`
- Belongs to groups: `sudo`, `user42`
- Root login via SSH is **disabled**
- New users can be added and assigned to groups during evaluation

---

### Password Policy

Configured via:
- `/etc/login.defs`
- `/etc/pam.d/common-password`

| Policy                   | Value                    |
|--------------------------|--------------------------|
| Max password age         | 30 days                  |
| Min password age         | 2 days                   |
| Password warning         | 7 days before expiration |
| Min password length      | 10 characters            |
| Uppercase & digit        | Required                 |
| Max repeat characters    | 3                        |
| Reject username in pass  | Enabled                  |
| Password delta (difok)   | 7                        |

---

### Sudo Configuration

Configured via `/etc/sudoers.d/<custom-file>`

| Option               | Description                                  |
|----------------------|----------------------------------------------|
| `passwd_tries=3`     | 3 password attempts allowed                  |
| `badpass_message`    | Custom error message on failed attempt       |
| `logfile`            | Logs sudo commands to `/var/log/sudo/`       |
| `log_input/output`   | Captures terminal input/output               |
| `requiretty`         | Requires TTY for sudo                        |
| `secure_path`        | Set to include only standard system paths    |

---

### Firewall Configuration

Configured using **UFW**:

```bash
$ sudo ufw enable
$ sudo ufw allow 4242
```
Only port 4242 (SSH) is open.

### SSH Configuration
Configured in /etc/ssh/sshd_config:

| Setting               | Value                                  |
|----------------------|----------------------------------------------|
| `Port`     | 4242                |
| `PermitRootLogin`    | no      |


### Monitoring Script
A `monitoring.sh` script is installed to:

- Display system stats every 10 minutes via cron
- Output to all TTYs using `wall`
Displays
- OS architecture and kernel
- Physical & virtual CPUs
- RAM and disk usage
- CPU load
- Last boot date
- LVM status
- TCP connections
- Logged-in users
- IP and MAC address
- Sudo command count

### Bonus Part
## WordPress (LLMP Stack)
Configured using:

- Lighttpd
- MariaDB
- PHP (php-cgi, php-mysql)

Steps included:
- Database and user creation
- Downloading and configuring WordPress
- Enabling Lighttpd fastcgi modules

Access via:
`http://<vm-ip>/`

### Additional Services
Added extra service(s) per subject flexibility. Ports configured appropriately in firewall.

### Disk Signature
Per subject instruction, a signature.txt is included in the root of the repository. The signature was generated from the virtual disk file using:

```bash
$ sha1sum <vm-name>.vdi
```

### License
This configuration project is free and open for reuse under the [MIT License](LICENSE).
