## These commands are for configuring a Juniper device using Junos OS via CLI (Command Line Interface):


```
Login: root
Password: root@123

1.	cli
2.	edit
3.	run show interfaces terse
4.	delete
5.	set system root-authentication plain-text-password
6.	commit
7.	run show interfaces terse
8.	set interfaces me0.0 family inet address 192.168.10.1/24
9.	set system services ssh
10.	commit
```




---

```markdown
# Junos CLI Configuration Guide

This document provides a step-by-step explanation of essential Junos OS commands used to configure and manage a Juniper device. Make sure you are logged in with proper credentials:

- **Login:** `root`
- **Password:** `root@123`

---

## 1. `cli`

This command switches from the shell environment (UNIX shell) to the Junos Command Line Interface (CLI). The CLI is where all device configurations and monitoring tasks are performed.

```sh
cli
```

---

## 2. `edit`

This command moves the CLI into **configuration mode**, allowing you to modify the device’s configuration.

```sh
edit
```

Once in configuration mode, the prompt changes to `#` and you can start setting or deleting configurations.

---

## 3. `run show interfaces terse`

This command is used **outside** configuration mode (or with `run` keyword inside it) to display a concise summary of all interfaces and their current status.

```sh
run show interfaces terse
```

Output includes:
- Interface names
- IP addresses (if any)
- Link status (up/down)

---

## 4. `delete`

Used within configuration mode to delete specific configuration entries.

```sh
delete <hierarchy>
```

Example:
```sh
delete system services ssh
```

This would delete the SSH service from the system configuration.

---

## 5. `set system root-authentication plain-text-password`

This sets the root user password using plain-text input (not recommended for production). After entering this command, the system will prompt you to type and confirm the new password.

```sh
set system root-authentication plain-text-password
```

---

## 6. `commit`

Applies all changes made in configuration mode to the running configuration. Until `commit` is issued, the changes remain in a candidate configuration.

```sh
commit
```

---

## 7. `run show interfaces terse`

Same as command 3, used to verify if the interface configurations (e.g., IP addresses) are correctly applied and active.

---

## 8. `set interfaces me0.0 family inet address 192.168.10.1/24`

Configures the management Ethernet interface `me0.0` with an IPv4 address.

```sh
set interfaces me0.0 family inet address 192.168.10.1/24
```

- `me0.0`: Logical interface for management
- `family inet`: IPv4 configuration
- `address 192.168.10.1/24`: Sets the IP address and subnet mask

---

## 9. `set system services ssh`

Enables SSH service on the device so that it can be managed remotely via Secure Shell.

```sh
set system services ssh
```

---

## 10. `commit`

Same as command 6. This is necessary to **apply** the recent configurations such as IP address assignment and SSH service.

---

## Summary

| Command | Description |
|--------|-------------|
| `cli` | Enters Junos CLI from the shell |
| `edit` | Enters configuration mode |
| `run show interfaces terse` | Shows status of interfaces concisely |
| `delete` | Deletes configuration entries |
| `set system root-authentication plain-text-password` | Sets root password |
| `commit` | Saves configuration changes |
| `set interfaces me0.0 ...` | Sets IP address to management interface |
| `set system services ssh` | Enables SSH access |

