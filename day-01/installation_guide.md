
# 🛠️ Ansible Installation Guide

This guide will walk you through installing Ansible on various platforms.

---

## ✅ Prerequisites

- A supported OS (Linux, macOS, Windows Subsystem for Linux (WSL))
- Python 3.9 or later
- Access to terminal/command line

---

## 📦 Install Ansible on Ubuntu / Debian

```bash
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y
```

🔍 Verify Installation:
```bash
ansible --version
```

---

## 📦 Install Ansible on CentOS / RHEL

```bash
sudo yum install epel-release -y
sudo yum install ansible -y
```

Or for RHEL 8+:

```bash
sudo dnf install epel-release
sudo dnf install ansible -y
```

---

## 🍏 Install Ansible on macOS (using Homebrew)

```bash
brew install ansible
```

---

## 🪟 Install Ansible on Windows (via WSL)

1. Enable **WSL** and install **Ubuntu** from Microsoft Store.
2. Open WSL terminal.
3. Follow Ubuntu installation steps listed above.

---

## 🐍 Install Ansible via pip (Python Package Manager)

If you prefer installing via Python pip:

```bash
python3 -m pip install --user ansible
```

🛠 Add pip binaries to your PATH if not already:
```bash
export PATH=$PATH:~/.local/bin
```

---

## 🔍 Verify Installation

```bash
ansible --version
```

Example output:
```
ansible [core 2.15.1]
  config file = /etc/ansible/ansible.cfg
  python version = 3.10.6
  jinja version = 3.1.2
```

---

## 📚 Next Steps

- ✅ Try your first command:
  ```bash
  ansible localhost -m ping
  ```
- ✅ Create your inventory file and write your first playbook.

---

## 🧰 Helpful Resources

- [Ansible Docs](https://docs.ansible.com/)
- [Ansible Galaxy](https://galaxy.ansible.com/)
- [Red Hat Ansible](https://www.ansible.com/)
