# Day 2: Ansible Ad-hoc Commands

Welcome to Day 2. Today, we'll explore Ansible Ad-hoc commands – quick, one-liner commands used for immediate tasks without writing full playbooks. We'll also cover essential concepts like passwordless authentication and Ansible inventory.

---

## 2.1 Passwordless Authentication (SSH Keys)

For Ansible to communicate with your managed nodes securely and efficiently, passwordless SSH authentication using SSH keys is highly recommended. This avoids the need to enter passwords repeatedly or store them in an insecure manner.

* **How it works:**
    1.  You generate a pair of SSH keys on your **Ansible control node**: a `private key` (kept secret) and a `public key` (can be shared). In case of AWS ec2, you download the .pem file while creating the instance.
    2.  The `public key` is copied to the `~/.ssh/authorized_keys` file on your **managed nodes**.
    3.  When Ansible tries to connect, the managed node challenges the control node. The control node responds by proving it possesses the corresponding private key.

* **Steps to set up (on your Ansible Control Node):**
    ## with instances having password
    1.  **Generate SSH Key Pair (if you don't have one):**
        ```bash
        ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
        ```
        * Press Enter to accept the default file location (`~/.ssh/id_rsa`).
        * You can set a passphrase for extra security, but for a learning environment, you might opt to leave it empty for simplicity.
        * This command creates `id_rsa` (private key) and `id_rsa.pub` (public key) in your `~/.ssh/` directory.

    2.  **Copy Public Key to Managed Node(s):**
        * **Using `ssh-copy-id` (recommended and easiest):**
            ```bash
            ssh-copy-id username@remote_host_ip
            ```
            * Replace `username` with the user on the remote host (e.g., `ec2-user`, `ubuntu`, `centos`).
            * Replace `remote_host_ip` with the IP address or hostname of your managed node.
            * You will be prompted for the remote user's password.

        * **Manually (if `ssh-copy-id` is not available or preferred):**
            ```bash
            cat ~/.ssh/id_rsa.pub | ssh username@remote_host_ip "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
            ```
            * Again, replace `username` and `remote_host_ip`. You will be prompted for the remote user's password.
        ---

    ## with instances having ssh keys (like ec2)

    ### Using Public Key

    ```
    ssh-copy-id -f "-o IdentityFile <PATH TO PEM FILE>" ubuntu@<INSTANCE-PUBLIC-IP>
    ```

    - ssh-copy-id: This is the command used to copy your public key to a remote machine.
    - -f: This flag forces the copying of keys, which can be useful if you have keys already set up and want to overwrite them.
    - "-o IdentityFile <PATH TO PEM FILE>": This option specifies the identity file (private key) to use for the connection. The -o flag passes this option to the underlying ssh command.
    - ubuntu@<INSTANCE-IP>: This is the username (ubuntu) and the IP address of the remote server you want to access.

    ### Using Password 

    - Go to the file `/etc/ssh/sshd_config.d/60-cloudimg-settings.conf`
    - Update `PasswordAuthentication yes`
    - Restart SSH -> `sudo systemctl restart ssh`

    3.  **Test the connection:**
        ```bash
        ssh username@remote_host_ip
        ```
        If successful, you should log in without being prompted for a password.

---

## 2.2 Ansible Inventory

The Ansible Inventory is a list of managed nodes that Ansible can connect to. It can be a simple text file or dynamically generated. It acts as the "source of truth" for the hosts Ansible manages.

* **Default Location:** `/etc/ansible/hosts` (if installed via package manager) or you can specify a custom inventory file (normally called inventory.ini placed anywhere) using the `-i` flag.
* **Format:** Typically in INI or YAML format. INI is simpler for basic inventories.

* **Example Inventory (`inventory.ini`):**

    ```ini
    # inventory.ini

    # --- Grouping hosts ---
    [webservers]
    ec2-user@web1.example.com
    ubuntu@192.168.1.10

    [dbservers]
    ec2-user@db1.example.com    
    ubuntu@192.168.1.20

    # --- Defining host variables ---
    [webservers:vars]
    ansible_user=ec2-user
    http_port=80
    max_clients=200

    # --- Specifying connection details for individual hosts ---
    # You can specify ansible_host if the hostname is different from the actual IP
    # Or ansible_user if it's different from the default
    node1 ansible_host=172.31.0.10 ansible_user=ubuntu
    node2 ansible_host=172.31.0.11 ansible_user=ec2-user

    # --- Using ranges ---
    [appservers]
    app[01:05].example.com # Expands to app01.example.com, app02.example.com, ... app05.example.com

    # --- All hosts implicitly belong to 'all' group ---
    ```

* **Key Inventory Variables:**
    * `ansible_host`: The host name or IP address to connect to.
    * `ansible_port`: The SSH port number (default is 22).
    * `ansible_user`: The username to connect as (e.g., `ec2-user`, `ubuntu`).
    * `ansible_ssh_private_key_file`: Path to the private key file.
    * `ansible_python_interpreter`: Path to the Python interpreter on the remote host (e.g., `/usr/bin/python3`).

---

## 2.3 Understanding Ad-hoc Commands and Their Usage

Ad-hoc commands are single-line commands executed directly from the command line using `ansible` command. They are useful for quick tasks, testing connectivity, or gathering information.

* **Basic Syntax:**
    ```bash
    ansible <pattern> -m <module_name> -a "<module_arguments>" -i <inventory_file>
    ```
    * `<pattern>`: Specifies which hosts from the inventory to target (e.g., `all`, `webservers`, `web1.example.com`, `webservers:!dbservers`).
    * `-m <module_name>`: The Ansible module to use (e.g., `ping`, `command`, `shell`, `apt`, `yum`, `service`).
    * `-a "<module_arguments>"`: Arguments to pass to the module.
    * `-i <inventory_file>`: (Optional) Path to your custom inventory file. If not specified, Ansible looks for `/etc/ansible/hosts`.
    * `-u <remote_user>`: (Optional) Specify the remote user if not defined in inventory or default.
    * `-k`: (Optional) Prompt for SSH password (if not using keys).
    * `-b`: (Optional) Run with `sudo` or `become` (escalate privileges).
    * `-K`: (Optional) Prompt for `sudo` password.

---

## 2.4 Examples of Common Ad-hoc Commands for System Management Tasks

Let's look at some practical examples. Assume you have an `inventory.ini` file set up and SSH keys configured.

1.  **Ping all hosts to test connectivity:**
    ```bash
    ansible all -m ping -i inventory.ini
    ```
    * Expected output: `SUCCESS => { "changed": false, "ping": "pong" }` for each host.

2.  **Run a shell command on `webservers` group:**
    ```bash
    ansible webservers -m shell -a "hostname -f" -i inventory.ini
    ```
    * This will show the fully qualified hostname of each web server.

3.  **Check disk usage on a specific host:**
    ```bash
    ansible web1.example.com -m command -a "df -h /" -i inventory.ini
    ```

4.  **Install `nginx` on `webservers` (requires `become`):**
    * **For Debian/Ubuntu-based systems:**
        ```bash
        ansible webservers -m apt -a "name=nginx state=present" -i inventory.ini -b
        ```
    * **For RHEL/CentOS-based systems:**
        ```bash
        ansible webservers -m yum -a "name=nginx state=present" -i inventory.ini -b
        ```

5.  **Ensure `nginx` service is running and enabled on boot:**
    ```bash
    ansible webservers -m service -a "name=nginx state=started enabled=yes" -i inventory.ini -b
    ```

6.  **Create a directory on `dbservers`:**
    ```bash
    ansible dbservers -m file -a "path=/opt/my_app/data state=directory mode=0755" -i inventory.ini -b
    ```

7.  **Copy a file from control node to `webservers`:**
    ```bash
    ansible webservers -m copy -a "src=/path/to/local/file.txt dest=/tmp/file.txt" -i inventory.ini
    ```
    * Replace `/path/to/local/file.txt` with an actual file on your Ansible control node.

8.  **Gather system facts (useful for debugging and understanding remote systems):**
    ```bash
    ansible all -m setup -i inventory.ini
    ```
    * This will return a large JSON output with detailed information about each host (OS, memory, network interfaces, etc.).

---

## 2.5 Exploring the Power of Ad-hoc Commands for Quick Tasks

Ad-hoc commands are incredibly powerful for:

* **Quick Checks:** Verify service status, disk space, process lists.
* **One-off Fixes:** Restart a service, create a temporary directory, modify a file.
* **Debugging:** Gather facts, test connectivity, run specific commands.
* **Prototyping:** Experiment with modules before integrating them into a playbook.

While powerful for immediate actions, remember that for complex, repeatable, and version-controlled automation, playbooks (which we'll cover tomorrow!) are the preferred method.

---

## Day 2 Checklist:

* [ ] Understand the importance and setup of passwordless SSH authentication.
* [ ] Create a basic Ansible inventory file (`inventory.ini`) with a few hosts and groups.
* [ ] Comprehend the basic syntax of Ansible ad-hoc commands.
* [ ] Practice running various ad-hoc commands for common system management tasks (ping, shell, package installation, service management, file operations).
* [ ] Understand when to use ad-hoc commands versus playbooks.

---

**Next Steps:**
Get ready for Day 3, where you'll write your very first Ansible Playbook!