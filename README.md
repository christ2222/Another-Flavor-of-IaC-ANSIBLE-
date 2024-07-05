# Ansible Controller and Client Setup

This project demonstrates the setup of an Ansible controller and four Ansible clients. The clients consist of two Amazon Linux 2 instances and two Ubuntu instances. The purpose of this project is to automate and manage the configuration of these instances using Ansible.
in this project client = worker
## Table of Contents
- [Prerequisites](#prerequisites)
- [Architecture](#architecture)
- [Setup Instructions](#setup-instructions)
  - [1. Setting up the Ansible Controller](#1-setting-up-the-ansible-controller)
  - [2. Configuring Ansible Clients](#2-configuring-ansible-clients)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)

## Prerequisites
- AWS Account
- 2 keys pairs : Ansible_controller_kp ; Ansible_worker_kp
- Ansible installed on the controller machine
- SSH access to all client instances
- Basic knowledge INI

## Architecture (ref. diagram)
The architecture of this setup includes:
- **Ansible Controller**: One ubuntu Instance: The central machine from which Ansible commands are executed.
- **Ansible Clients**: Four machines (two Amazon Linux 2023 instances and two Ubuntu instances) that will be managed by the Ansible controller.

## Setup Instructions

### 1. Setting up the Ansible Controller
1. **Launch an EC2 Instance**: Launch an Ubuntu EC2 instance to serve as the Ansible controller. Ensure it has Ansible installed.
2. **Check if python3 install**:
     ```sh
     python3 --version
      ```
     if not installed , install:
   - For Ubuntu clients:
     ```sh
     sudo apt update
     sudo apt install -y python3
     ```
4. **Change the Hostname**:
Set the hostname of the instance to <controller>:
```sh
sudo hostnamectl set-hostname <controller>
 ```
Exit and reconnect (SSH) to the instance for the new hostname to appear in the path.
4. **Install Ansible**:
   ```sh
   sudo apt update
   sudo apt install software-properties-common
   sudo add-apt-repository --yes --update ppa:ansible/ansible
   sudo apt install ansible
   ```
4. **Configure SSH Access**:
   - Generate an SSH key pair if you don't have one:
     ```sh
     ssh-keygen -t rsa
     ```
   - Copy the public key to each client instance:
     ```sh
     ssh-copy-id user@client-ip
     ```

### 2. Configuring Ansible Clients
1. **Launch EC2 Instances**: Launch four EC2 instances, two with Amazon Linux 2023 and two with Ubuntu.
2. **Update and Install Required Packages**:
   -First check if Python iinstalled,
     ```sh
     python3 --version
      ```
     if not installed.
   - For Amazon Linux 2023 clients:
     ```sh
     sudo yum update -y
     sudo yum install -y python3
     ```
   - For Ubuntu clients:
     ```sh
     sudo apt update
     sudo apt install -y python3
     ```
4. **Ensure SSH Access**: Ensure that the Ansible controller can SSH into the clients with your key pair created in the prerequisites.
5. **Change the Hostname**:
Set the hostname of the instance to <worker>:
```sh
sudo hostnamectl set-hostname <worker>
 ```
Exit and reconnect (SSH) to the instance for the new hostname to appear in the path.

### 3. Configuring Ansible Inventory
1. **Edit `hosts` file**: On the Ansible controller, create or edit the `hosts` file to include the client details:
   ```ini
   [amazon_linux]
   amazon_client1 ansible_host=client1-ip ansible_user=ec2-user
   amazon_client2 ansible_host=client2-ip ansible_user=ec2-user

   [ubuntu]
   ubuntu_client1 ansible_host=client3-ip ansible_user=ubuntu
   ubuntu_client2 ansible_host=client4-ip ansible_user=ubuntu
   ```

## Usage
- Test connectivity:
  ```sh
  ansible all -m ping
  ```
  ```sh
  ssh ec2-user@<ip-address>
  ```
  ```sh
  ssh ubuntu@<ip-address>
  ```

```

## Contributing
Contributions are welcome! Please fork this repository and submit a pull request for any enhancements or fixes.

## License
This project is licensed under the MIT License. See the LICENSE file for details.

---

Feel free to customize this README file to better suit the specifics of your project. If you have any questions or need further assistance, please don't hesitate to ask!
