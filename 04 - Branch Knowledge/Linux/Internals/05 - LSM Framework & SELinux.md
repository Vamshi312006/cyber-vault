---
id: "mod-nix-lsm-framework"
title: "Linux Security Modules (LSM) Framework & SELinux Architecture"
domain: "Domain-01"
branch: "linux-kernel"
type: "module"
maintainer: "Cyber Act Engineering Team"
last_audited: "2026-07-29"
---

# Linux Security Modules (LSM) Framework & SELinux Architecture

## 1. Overview & Purpose
Discretionary Access Control (DAC) in Linux (standard POSIX file permissions: `rwxrwxrwx`) relies entirely on file ownership; if a process runs as `root`, it bypasses all DAC checks. **Mandatory Access Control (MAC)** solves this vulnerability by enforcing system-wide security policies regardless of user UID.

This module details the Linux Security Modules (LSM) hook architecture, SELinux Mandatory Access Control (Type Enforcement, Security Contexts, AVC Caching), AppArmor path-based profiling, and modern Landlock unprivileged sandboxing. Understanding LSM is essential for enterprise OS hardening, container isolation, and Mandatory Access Control policy engineering.

---

## 2. Architecture & Key Components

```mermaid
graph TD
    subgraph Ring 3 (User Space)
        APP["Application (e.g., /usr/sbin/httpd)"]
    end

    subgraph Ring 0 (Kernel Space System Call Execution)
        SYSCALL["System Call Entry (e.g., sys_openat)"]
        DAC["DAC Permission Check (Standard POSIX Owner/Group Permissions)"]
        LSM_HOOK["LSM Security Hook (security_file_open)"]

        subgraph Active LSM Modules (Stackable LSMs)
            SELINUX["SELinux (Type Enforcement & Security Context Check)"]
            APPARMOR["AppArmor (Path Profile Check)"]
            YAMA["Yama (Ptrace Scope Restrictions)"]
            LANDLOCK["Landlock (Unprivileged Process Sandboxing)"]
        end

        OBJECT["Target Resource (e.g., /etc/shadow)"]

        SYSCALL --> DAC
        DAC -->|Passes DAC| LSM_HOOK
        LSM_HOOK --> SELINUX
        LSM_HOOK --> APPARMOR
        LSM_HOOK --> YAMA
        LSM_HOOK --> LANDLOCK
        LSM_HOOK -->|Passes LSM| OBJECT
    end

    APP --> SYSCALL
```

---

## 3. Detailed Mechanics & Internal Structures

### 3.1 The LSM Hook Architecture
LSM provides a framework of security hooks embedded at critical control points across kernel subsystems (Filesystems, Processes, Sockets, IPC, Modules):
- `security_bprm_check()`: Called during `execve()` to inspect binaries before execution.
- `security_file_open()`: Called during `openat()` before granting file access.
- `security_socket_connect()`: Called before establishing network socket connections.

If standard Linux DAC permissions pass, the kernel executes registered LSM hooks. If any LSM module returns non-zero (`-EACCES`), access is denied.

---

### 3.2 SELinux Architecture & Security Contexts
SELinux (Security-Enhanced Linux) implements Mandatory Access Control using Type Enforcement (TE).

#### Security Context Format:
Every process (subject) and file/socket (object) possesses an extended attribute security context:
`user : role : type : sensitivity_level`
- *Example Process Context*: `system_u:system_r:httpd_t:s0`
- *Example Target File Context*: `system_u:object_r:httpd_sys_content_t:s0`

#### Type Enforcement Rule Format:
```text
allow httpd_t httpd_sys_content_t : file { read getattr open };
```
*Meaning*: Processes running in domain `httpd_t` are allowed to `read`, `getattr`, and `open` files labeled with type `httpd_sys_content_t`. If `httpd_t` attempts to read `/etc/shadow` (labeled `shadow_t`), SELinux blocks the access **even if `httpd_t` runs as root**.

#### Access Vector Cache (AVC):
To prevent slow policy evaluation, SELinux caches decision rules in the Access Vector Cache (AVC).

---

### 3.3 AppArmor Path-Based Protection
Unlike SELinux (which attaches security labels to filesystem inodes), **AppArmor** enforces MAC rules based on absolute file paths (e.g., `/etc/nginx/nginx.conf r,`). AppArmor profiles are easier to configure and widely used in Ubuntu and Debian environments.

---

## 4. Security Implications & Boundary Controls

- **Root Containment**: LSM ensures that a compromised service running as `root` (e.g., an exploited web server or container daemon) cannot modify host system binaries, read sensitive database files, or load unauthorized kernel modules.
- **SELinux Enforcing Modes**:
  - `Enforcing` (`1`): SELinux blocks unauthorized actions and logs violations.
  - `Permissive` (`0`): SELinux permits actions but logs AVC denial warnings.
  - `Disabled`: SELinux kernel hooks completely inactive.

---

## 5. Attack Vectors & Bypasses

1. **SELinux Relabeling Exploits**:
   If an application running in a broad domain (or possess `mac_admin` capability) can execute `chcon` or `setfiles`, it can relabel arbitrary files (e.g., changing `/etc/shadow` label to `httpd_sys_content_t`), bypassing Type Enforcement.
2. **Disabling SELinux via Vulnerable Kernel Write**:
   Kernel exploits overwrite the global kernel variable `selinux_enforcing` (or disable LSM hooks in kernel memory) to disable SELinux enforcement dynamically.

---

## 6. Defense & Telemetry Verification

### Telemetry Tracing Sources:
- **Auditd AVC Denial Logs**: `/var/log/audit/audit.log` (`type=AVC`).
- **Kernel Log Buffer**: `dmesg | grep -i avc`.

### Example AVC Denial Audit Log Entry:
```text
type=AVC msg=audit(1690000000.123:456): avc: denied { read } for pid=1420 comm="httpd" name="shadow" dev="sda1" ino=131078 scontext=system_u:system_r:httpd_t:s0 tcontext=system_u:object_r:shadow_t:s0 tclass=file permissive=0
```

---

## 7. Engineering & Hands-On Implementation

### Auditing & Changing SELinux Modes:
```bash
# Check current SELinux status
sestatus

# Get security context of a process or file
ps -eZ | grep httpd
ls -Z /etc/shadow

# Temporarily set to Permissive mode for troubleshooting
sudo setenforce 0
```

### Generating SELinux Policy Modules from Audit Logs:
```bash
# Analyze AVC denials and generate missing policy rule module
sudo audit2allow -a -M my_custom_policy
sudo semodule -i my_custom_policy.pp
```

---

## 8. Troubleshooting & Diagnostics

| Symptom | Root Cause | Remediation / Diagnostic Command |
| :--- | :--- | :--- |
| Application fails with `Permission denied` despite `chmod 777`. | LSM (SELinux or AppArmor) MAC policy blocking access. | Inspect `dmesg \| grep AVC` or `journalctl -u apparmor` to identify blocked LSM rule. |
| SELinux context lost after file restore. | File restored without maintaining context extended attributes (`xattr`). | Execute `restorecon -v /path/to/file` to reset correct policy context. |

---

## 9. References
- National Security Agency (NSA), *SELinux Architecture & Policy Design Guide*.
- Linux Kernel Documentation: *Linux Security Modules (LSM) Framework*.
