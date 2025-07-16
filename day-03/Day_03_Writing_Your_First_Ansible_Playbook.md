# Day 3: Writing Your First Ansible Playbook

Welcome to Day 3. Today is a pivotal day as you'll transition from ad-hoc commands to writing your first Ansible Playbook. Playbooks are the heart of Ansible automation, allowing you to define complex, repeatable, and organized workflows.

---

## 3.1 Understanding YAML Basics and Ansible Playbook Structure

Ansible playbooks are written in YAML (YAML Ain't Markup Language), a human-friendly data serialization standard. Its simplicity makes it ideal for configuration files.

* **YAML Basics:**
    * **Indentation Matters:** YAML uses indentation (spaces, not tabs!) to denote structure and hierarchy. Consistent indentation is crucial.
    * **Key-Value Pairs:** Data is represented as `key: value`.
    * **Lists (Sequences):** Items in a list are denoted by a hyphen (`- `) followed by a space.
        ```yaml
        - item1
        - item2
        ```
    * **Dictionaries (Mappings/Objects):** Key-value pairs.
        ```yaml
        my_dict:
          key1: value1
          key2: value2
        ```
    * **Strings:** Usually unquoted unless they contain special characters (like `:`, `[`, `]`, `{`, `}`).
    * **Comments:** Begin with `#`.

    * **Example YAML Snippet:**
        ```yaml
        # This is a comment
        name: Praveen Kumar Bharti
        age: 30
        isWorking: True
        hobbies:
          - reading
          - coding
          - hiking
        address:
          street: 123, Pune
          city: Anytown
          zip: "12345" 
        ```

* **Ansible Playbook Structure:**
    An Ansible playbook is a text file, usually ending with `.yml` or `.yaml`. It consists of one or more "plays".

    ```yaml
    ---
    # This is an Ansible Playbook
    # All playbooks start with '---' (optional but good practice)

    - name: My First Play
      hosts: webservers        # Target hosts from your inventory
      become: yes              # Run tasks with escalated privileges (sudo)
      gather_facts: yes        # Gather system facts (default: yes)

      tasks:                   # A list of tasks to be executed in order
        - name: Install Nginx web server
          ansible.builtin.package: # Using the package module (apt/yum/dnf based on OS)
            name: nginx
            state: present

        - name: Ensure Nginx service is running and enabled
          ansible.builtin.service:
            name: nginx
            state: started
            enabled: true

        - name: Deploy a simple HTML file
          ansible.builtin.copy:
            src: index.html     # Path to file on control node
            dest: /var/www/html/index.html # Destination on managed node
            mode: '0644'

    - name: Second Play (optional, for another set of hosts/tasks)
      hosts: dbservers
      become: yes
      tasks:
        - name: Install PostgreSQL
          ansible.builtin.package:
            name: postgresql
            state: present
    ```

---

## 3.2 Introduction to Ansible Structure: Playbook, Play, Modules, Tasks, and Collections

Let's break down the key components of an Ansible playbook:

* **Playbook:**
    * The entire YAML file.
    * Contains one or more "plays". Each play contains one or more "tasks".
    * Defines the overall automation workflow.

* **Play:**
    * A block within a playbook (starting with `- name:`, `hosts:`, etc.).
    * Targets a specific group of `hosts` (or `all` hosts) from your inventory.
    * Can have global settings for that specific play, like `become: yes` (to run tasks as root/sudo).
    * Contains a list of `tasks`.
    * The `name` attribute is highly recommended for readability and debugging.

* **Modules:**
    * Ansible's functional units. These are small programs (often Python scripts) that execute specific actions on managed nodes.
    * Examples: `ansible.builtin.package` (for installing/managing packages), `ansible.builtin.service` (for managing services), `ansible.builtin.copy` (for copying files), `ansible.builtin.command`, `ansible.builtin.shell`, `ansible.builtin.file`, etc.
    * Modules are *idempotent* where possible, meaning running them multiple times will yield the same result without causing unintended changes if the desired state is already met.

* **Tasks:**
    * A single action performed by a module within a play.
    * Each task starts with a `name:` attribute (crucial for clear output during playbook execution).
    * Followed by the module name and its arguments.
    * Tasks are executed sequentially within a play. If a task fails, the entire play (by default) stops for that host.

* **Collections:**
    * Since Ansible 2.9, modules and plugins are bundled into "Collections."
    * Collections are a way to distribute and consume Ansible content (modules, plugins, roles, playbooks).
    * They are namespaced (e.g., `ansible.builtin` for core modules, `amazon.aws` for AWS-specific modules).
    * You can use the fully qualified collection name (FQCN) like `ansible.builtin.package` or rely on default search paths. For clarity, especially when new to Ansible, using the FQCN is good practice.

---

## 3.3 Hands-on: Writing a Playbook to Install Apache2 and Deploy a Static App on AWS

Let's put theory into practice. We'll create a playbook to:
1.  Install Apache2 (or `httpd` for RHEL/CentOS).
2.  Ensure Apache service is running and enabled on boot.
3.  Deploy a simple `index.html` file to the web server's document root.

**Prerequisites:**
* An AWS EC2 instance running Ubuntu (recommended for this example) or Amazon Linux/RHEL.
* SSH access configured with passwordless authentication from your Ansible control node to the EC2 instance.
* Your `inventory.ini` file should list your EC2 instance(s). Example:
    ```ini
    # inventory.ini
    [aws_webservers]
    your_ec2_public_ip_or_hostname ansible_user=ubuntu # or ec2-user for Amazon Linux
    ```

**Step 1: Create an `index.html` file on your Ansible Control Node**
In the same directory where you'll create your playbook, create a file named `index.html` with the following content:

```html
<!DOCTYPE html>
<html>
<head>
    <title>My First Ansible App</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; margin-top: 50px; }
        h1 { color: #333; }
        p { color: #666; }
    </style>
</head>
<body>
    <h1>Hello from Ansible!</h1>
    <p>This page was deployed using my first Ansible Playbook.</p>
    <p>Current server time: {{ ansible_date_time.iso8601_micro }}</p>
</body>
</html>