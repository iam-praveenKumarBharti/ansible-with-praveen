# Ansible: Understanding `gather_facts`

## ✅ What is `gather_facts`?

In Ansible, `gather_facts` is a setting used in a playbook to **collect detailed system information** (called "facts") from the managed hosts **before executing tasks**. These facts are automatically pulled using the `setup` module.

---

## 🧠 What Does It Collect?

When `gather_facts: yes` is set (default), Ansible collects **facts** such as:

| Fact Name                  | Description                           |
|---------------------------|---------------------------------------|
| `ansible_os_family`       | OS family (e.g., RedHat, Debian)      |
| `ansible_distribution`    | OS name (e.g., Ubuntu, CentOS)        |
| `ansible_distribution_version` | OS version                     |
| `ansible_hostname`        | Hostname of the machine               |
| `ansible_processor`       | CPU details                           |
| `ansible_memory_mb`       | Total and free memory                 |
| `ansible_interfaces`      | Network interfaces                    |
| `ansible_all_ipv4_addresses` | IP addresses                     |
| `ansible_mounts`          | Mounted volumes                       |
| `ansible_env`             | Environment variables                 |

To see facts:
```bash
ansible all -m setup -i inventory.ini
```

---

## 🔧 Syntax in a Playbook
```yaml
- name: Gather system facts and do something
  hosts: all
  gather_facts: yes
  tasks:
    - name: Print OS family
      debug:
        msg: "This host is from the {{ ansible_facts['os_family'] }} family"
```

---

## 🚫 When to Disable It

If you don’t need system info, especially to **speed up execution**, you can skip it:
```yaml
gather_facts: no
```

Manually gather specific facts when required:
```yaml
- name: Gather only network facts
  ansible.builtin.setup:
    gather_subset:
      - network
```

---

## 🎯 Use Cases
- Conditional tasks based on OS (`ansible_os_family`)
- Dynamic variable loading (e.g., templates or roles based on OS)
- Targeted deployments (e.g., RedHat vs Debian)
- Debugging/monitoring/logging system configuration

---

Let me know if you want a filtered `setup` example or how to write custom facts!

