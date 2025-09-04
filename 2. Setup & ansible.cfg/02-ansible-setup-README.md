
# Ansible Setup on AWS (Lab Environment)

This guide helps you set up a complete Ansible lab environment using 4 EC2 instances — one as the **Ansible Controller (Master)** and three as **Managed Nodes (Workers)**. The setup leverages SSH for communication and demonstrates agentless automation.

---

## 1. Prerequisites

- AWS account with access to launch EC2 instances.
- Basic knowledge of Linux commands and SSH.
- Key pair (PEM file) downloaded for SSH access.
- All 4 EC2s must be in the same VPC and region for ease of SSH communication.

---

## 2. Architecture

| Role           | Description                         |
|----------------|-------------------------------------|
| **Master Node**| Control node where Ansible is installed. |
| **Worker Nodes**| Managed nodes where tasks will be executed. |

- Master connects to workers over **SSH** (default port 22).
- No agent is required on worker nodes.
- Only the master node needs to have **Ansible installed**.
- Worker nodes must have **Python** (comes pre-installed in most AMIs).

---

## 3. Launch EC2 Instances

Launch 4 EC2 instances using Amazon Linux 2 or Ubuntu.

- 1 Master Node
- 3 Worker Nodes

For each instance:

- Choose instance type: `t2.micro` (Free tier eligible)
- Use same key pair for all instances
- Assign static IP (Elastic IP) for easier SSH
- Open **port 22** in the security group for SSH access
- Attach IAM role (if needed for dynamic inventory later)

---

## 4. Connect to EC2 Instances

From your local terminal, SSH into the master node:

```bash
ssh -i "your-key.pem" ec2-user@<Master-IP>
```

Once on the master, test SSH access to all worker nodes:

```bash
ssh ec2-user@<Worker1-IP>
ssh ec2-user@<Worker2-IP>
ssh ec2-user@<Worker3-IP>
```

If this works without password prompts, you're good to go.

---

## 5. Install Ansible on Master Node

Update and install dependencies:

```bash
sudo yum update -y   # For Amazon Linux
sudo yum install -y python3 python3-pip git
sudo pip3 install ansible
```

Verify installation:

```bash
ansible --version
```

> You should see the installed Ansible version and Python path.

---

## 6. Set Up SSH Access (Passwordless)

On the master node:

```bash
ssh-keygen -t rsa
```

Press Enter through all prompts (no passphrase).

Copy public key to all worker nodes:

```bash
ssh-copy-id ec2-user@<Worker1-IP>
ssh-copy-id ec2-user@<Worker2-IP>
ssh-copy-id ec2-user@<Worker3-IP>
```

---

## 7. Configure Ansible Inventory

Create an inventory file:

```bash
sudo mkdir /etc/ansible
sudo nano /etc/ansible/hosts
```

Example contents:

```ini
[web]
192.168.1.10
192.168.1.11

[db]
192.168.1.12

[all:vars]
ansible_user=ec2-user
ansible_ssh_private_key_file=~/your-key.pem
```

---

## 8. Test Ansible Connection

Run a ping test to all hosts:

```bash
ansible all -m ping
```

Expected output:

```
192.168.1.10 | SUCCESS => {"changed": false, "ping": "pong"}
...
```

---

## 9. Troubleshooting Tips

- Ensure port 22 is open on all worker nodes.
- If SSH key errors occur, delete `~/.ssh/known_hosts` and try again.
- Use verbose flag `-vvv` to debug Ansible commands.

---

## ✅ Next Steps

You are now ready to start running playbooks from the master node. Create `.yml` playbooks and target inventory groups to automate tasks like:

- Installing packages
- Creating users
- Configuring services

---
