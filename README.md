# ansible-by-praveen

Welcome to the **Ansible by Praveen Repository**. This repository is designed to help DevOps engineers to learn and practice Ansible, an open-source automation tool used for configuration management, application deployment and orchestration. Whether you are a beginner or looking to enhance your Ansible skills, this repository will serve as a comprehensive guide.

Note: If you found the content helpful, please consider giving this repository a star ⭐
---

## Repository Structure

This 11-day training program is designed to take you from a beginner to a proficient Ansible user, covering essential concepts and practical applications for automation.

---

## Day 1: Introduction to Ansible and Getting Started

* **Overview of Ansible**: What it is, its advantages, and why it's a powerful automation tool.
* **Comparison**: How Ansible stands out against traditional Shell and Python scripting for automation.
* **Installation**: Step-by-step guide to installing Ansible on various platforms.
* **IDE Setup**: Configuring VS Code with relevant plugins for an efficient Ansible development environment.

---

## Day 2: Ansible Ad-hoc Commands

* **Passwordless Authentication**: Setting up SSH for seamless, secure connections.
* **Ansible Inventory**: Understanding how to define and organize your managed hosts.
* **Ad-hoc Commands**: Exploring the syntax and practical usage of ad-hoc commands for quick, one-off tasks.
* **Examples**: Common ad-hoc commands for system management and their immediate impact.

---

## Day 3: Writing Your First Ansible Playbook

* **YAML Basics**: Essential YAML syntax for writing clean and readable playbooks.
* **Ansible Structure**: Deep dive into Playbooks, Plays, Modules, Tasks, and Collections.
* **Hands-on**: Creating a playbook to install Apache2 and deploy a static web application on an AWS instance.

---

## Day 4: Understanding Ansible Roles

* **What are Roles?**: Introduction to Ansible roles and their benefits for modularity and reusability.
* **Folder Structure**: Exploring the standardized directory structure of Ansible roles.
* **Roles vs. Playbooks**: Comparing their usage and understanding when to choose one over the other.
* **Hands-on**: Developing a simple role and integrating it into a playbook.

---

## Day 5: Deep Dive into Ansible Roles with Demo

* **Ansible Galaxy**: Discovering and utilizing pre-built roles from the Ansible Galaxy community.
* **Importing & Installing**: Practical steps for importing and installing roles from Ansible Galaxy.
* **DEMO**: Advanced role usage through a real-world project example, demonstrating best practices for organizing roles and playbook structure.

---

## Day 6: Ansible Variables and Precedence

* **AWS Resource Creation**: Using Ansible Collections to provision AWS resources.
* **Ansible Variables**: Understanding variable scope and defining variables with practical examples.
* **Jinja2 Templating**: Leveraging advanced templating features for dynamic content generation.
* **Variable Precedence**: How Ansible resolves conflicts when variables are defined in multiple locations.
* **Hands-on**: Applying variables within playbooks and roles for flexible automation.

---

## Day 7: Ansible Conditionals and Loops

* **Conditionals**: Controlling task execution based on specific conditions using `when` statements.
* **Loops**: Implementing loops (`with_items`, `with_dict`, etc.) for repetitive tasks efficiently.
* **Practical Examples**: Demonstrating real-world scenarios where conditionals and loops enhance playbook logic.

---

## Day 8: Error Handling in Ansible

* **Dealing with Errors**: Strategies for identifying and managing errors and failures in Ansible playbooks.
* **Error Handling Techniques**: Exploring `failed_when`, `changed_when`, `ignore_errors`, and `block/rescue` for robust automation.
* **Demonstration**: Practical scenarios showcasing effective error handling to ensure playbook resilience.

---

## Day 9: Ansible Vault for Security

* **Ansible Vault**: Understanding its importance in securing sensitive data within your Ansible projects.
* **Encryption & Decryption**: Hands-on practice encrypting and decrypting files using Ansible Vault.
* **Best Practices**: Strategies for securely managing secrets and sensitive information.

---

## Day 10: Policy as Code with Ansible

* **Introduction to Policy as Code**: Concepts and benefits of defining infrastructure and security policies in a machine-readable format.
* **Implementing Policies with Ansible**: Using Ansible to enforce configuration policies and compliance.
* **Practical Use Cases**: Examples of how Ansible can be used to audit and maintain desired states across your infrastructure.

---

## Day 11: Network Automation using Ansible

* **Ansible for Networking**: Introduction to using Ansible for managing network devices and infrastructure.
* **Network Modules**: Exploring common Ansible modules for network automation tasks (e.g., configuring routers, switches).
* **Practical Scenarios**: Hands-on examples of automating network configurations and operations.

---

## Prerequisites

Before diving into this repository, ensure you have the following:

- Basic understanding of Linux/Unix systems.
- Python installed on your local system.
- Ansible installed (follow the instructions in `day-01\installation_guide.md`).

---

## Getting Started

### 1. Clone the Repository

```bash
# Clone the repository
$ git clone https://github.com/iam-praveenKumarBharti/ansible-with-praveen.git

# Navigate into the repository
$ cd ansible-with-praveen
```

---

## Contribution Guidelines

We welcome contributions! Follow these steps to contribute:

1. Fork the repository.
2. Create a feature branch:
   ```bash
   $ git checkout -b feature/your-feature
   ```
3. Commit your changes:
   ```bash
   $ git commit -m "Add your message here"
   ```
4. Push your branch and create a Pull Request (PR).

For more details, refer to [CONTRIBUTING.md](CONTRIBUTING.md).

---

## License

This repository is open-sourced. Feel free to use and share it.

---

## Support

If you have any questions or need support, feel free to open an issue or contact the repository maintainer.