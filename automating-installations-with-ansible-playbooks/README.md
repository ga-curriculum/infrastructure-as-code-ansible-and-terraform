<h1>
  <span class="headline">Infrastructure As Code: Ansible & Terraform</span>
  <span class="subhead">Automating Installations with Ansible Playbooks</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to automate the installation of tools and dependencies using an Ansible playbook.

## Automating Installations with Ansible Playbooks

In this lesson, you'll learn how to use Ansible, a powerful automation tool, to install essential utilities that will be used in upcoming labs. Specifically, you’ll set up:

- The **AWS CLI** for managing AWS resources.
- **Terraform** for creating and managing cloud infrastructure.

You'll also explore an Ansible playbook, understand its syntax, and execute it to automate the installation process.

## What Are Ansible Playbooks?

Ansible uses **playbooks**, which are structured YAML files, to define the steps required to automate tasks.

### Why Are They Called _"Playbooks"_?

The term "playbook" comes from sports, where a playbook outlines a sequence of plays or strategies for the team to execute. Similarly, in Ansible, a playbook is a series of steps or "plays" that a system administrator or developer wants to automate, like installing software, configuring servers, or deploying applications.

### Key Components of a Playbook

| Component       | Description                                                                                        |
| --------------- | -------------------------------------------------------------------------------------------------- |
| **Hosts**       | Defines the systems where the tasks will run.                                                      |
| **Tasks**       | Individual steps executed to achieve the desired state.                                            |
| **Modules**     | Pre-built units of work (e.g., install a package, copy a file) that are reusable and customizable. |
| **YAML Format** | Playbooks use YAML, a human-readable data format, to make them easy to write and understand.       |

In this lesson, you'll review and execute a playbook to see how these concepts work in practice.

## 1. Install Ansible

First, we'll need to install Ansible on your machine. Open your terminal and run the following commands.

```bash
sudo amazon-linux-extras install epel -y
```

This command installs the EPEL (Extra Packages for Enterprise Linux) repository on your Amazon Linux instance. EPEL provides additional open-source packages that are not available in the standard Amazon Linux repositories. Many popular software packages, like Ansible, are provided through EPEL.

```bash
sudo yum -y install ansible
```

This command installs Ansible on your Amazon Linux instance.

### Verify the Installation

Check that Ansible has been installed and meets the version requirements:

```bash
ansible --version
```

Ensure the version is **2.9.XX** or later.

## 2. Install the Visual Studio Code Ansible Extension

Enhance your development environment by adding the Red Hat Ansible extension to Visual Studio Code:

1. Open **VS Code**.

2. Press `Ctrl+P` (Windows/Linux) or `Command+P` (Mac) and type:

   ```plaintext
   ext install redhat.ansible
   ```

   <br>

    <img src="./assets/install-extension.png" alt="Installing VS Code Extension" style="width:600px;"/>

    <br>

3. Alternatively, go to the **Extensions** tab in VS Code, search for “Ansible” by **Red Hat**, and click **Install**.
4. Restart **VS Code** after installation.

## 3. Review the Ansible Playbook

### Clone the starter code

Download the provided starter code for this lesson:

```bash
git clone <repository-url>
```

### Open the Playbook and Explore

Let's get started by launching **Visual Studio Code** and opening the repository you just cloned:

```bash
cd <repository-folder>
code .
```

Once inside, locate and open the `playbook.yml` file. This is where the magic happens!

This specific playbook contains a series of tasks that will automatically:

- **Install the AWS CLI**
- **Install Terraform**

Take some time to explore the playbook. You’ll see how tasks are defined, and how Ansible makes it easy to automate complex processes with just a few lines of code.

To get more familiar with the syntax and structure of an Ansible playbook, check out these resources:

- [Ansible Playbook Syntax Documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_intro.html#playbook-syntax)
- [Ansible Plugins Documentation](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/index.html#plugins-in-ansible-builtin)

### Playbook code review

Let’s examine the `playbook.yml`:

```yaml
---
- hosts: localhost
   pre_tasks:

  - name: Install dependencies
    ansible.builtin.package:
    name:
  - yum-utils
  - shadow-utils
  - amazon-linux-extras
    become: true

  post_tasks:

  - name: Download AWS CLI
    ansible.builtin.get_url:
    url: https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip
    dest: ./awscliv2.zip

    - name: Unzip AWS CLI
      ansible.builtin.unarchive:
      src: ./awscliv2.zip
      dest: .

    - name: Install AWS CLI
      ansible.builtin.command: ./aws/install --update
      become: true

    - name: Add HashiCorp Repos
      ansible.builtin.command: yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
      become: true

    - name: Install Terraform
      ansible.builtin.package:
      name: - terraform
      become: true
```

## 4. Execute the Playbook

Now that you've explored the playbook, it’s time to run it! Execute the following command to start the automation process:

```bash
ansible-playbook playbook.yml
```

### What does `ansible-playbook` do?

The `ansible-playbook` command executes the tasks defined in the playbook (`playbook.yml`). It automates each step in sequence, giving us consistent and error-free installations.

tktk talk about documents installed and maybe share a photo

## Level Up

Explore Ansible further by extending the playbook:

1. Think of additional tasks to automate, such as:

   - Installing another package.
   - Creating a specific configuration file.

2. Add your tasks to the playbook.

3. Re-run the updated playbook:

```bash
ansible-playbook playbook.yml
```

Experiment with these changes and see how Ansible simplifies repetitive setup tasks!
