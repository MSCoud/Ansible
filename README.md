
# 🚀 Ansible Playbook: Deploy 2048 Game with Nginx

This repository contains an Ansible playbook to automate the installation and configuration of Nginx, clone the 2048 game from GitHub, and serve it locally on a Linux machine (e.g., Ubuntu).

## 📆 Prerequisites

Make sure you're running this on a Debian-based system and you have root or sudo privileges.

## 🔧 Installation Steps

### 1. Install pipx

```bash
apt update
apt install pipx -y
pipx ensurepath
source ~/.bashrc
```

### 2. Install Ansible using pipx

```bash
pipx install ansible ansible-core
```

### 3. Verify the Ansible installation

```bash
ansible --version
```

## 🗂️ Inventory Setup

Create a file named `inventory.yml`:

```yaml
all:
  children:
    web:
      hosts:
        localhost:
```

## 📄 Playbook Content

Create a file named `playbook.yml`:

```yaml
- name: Configuration and deployment of 2048 on nginx server
  hosts: web
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
        update_cache: yes

    - name: Install Git
      apt:
        name: git
        state: present

    - name: Clone the web app code
      git:
        repo: https://github.com/ttwthomas/2048.git
        dest: /var/www/2048
        update: yes
        version: main
        force: yes

    - name: Update Nginx root directory
      replace:
        path: /etc/nginx/sites-enabled/default
        regexp: 'root /var/www/html;'
        replace: 'root /var/www/2048/app;'

    - name: Set correct permissions on the website directory
      file:
        path: /var/www/2048/app
        mode: '0755'
        recurse: yes

    - name: Restart Nginx
      service:
        name: nginx
        state: restarted
```

## ▶️ Run the Playbook

```bash
ansible-playbook -i ./inventory.yml playbook.yml
```

## ✅ Test the Setup

After running the playbook:

1. Test with curl:
   ```bash
   curl http://localhost/index.html
   ```

2. Or open in your browser:
   ```
   http://localhost/helloworld.html
   ```

## 📁 File Structure

```
.
├── inventory.yml
├── playbook.yml
└── README.md
```

## 📜 License

This project is licensed under the MIT License.

