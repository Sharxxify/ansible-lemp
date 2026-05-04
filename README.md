# Ansible LEMP + WordPress Deployment

Automated deployment of a production-grade LEMP stack (Linux, Nginx, MySQL, PHP) with WordPress using Ansible. Reduces a 30-minute manual server setup to a single command.

---

## What It Does

- Installs and configures **Nginx** as the web server
- Installs and secures **MySQL** with a dedicated WordPress database and user
- Installs **PHP 8.1 FPM** with all required modules
- Downloads and deploys **WordPress** with a fully generated `wp-config.php`
- Sets correct **file permissions and ownership**
- Verifies all services are running and prints a **deployment summary**

---

## Project Structure

```
ansible-lemp/
├── ansible.cfg
├── inventory.ini
├── site.yml
└── roles/
    ├── nginx/
    │   ├── tasks/main.yml
    │   ├── handlers/main.yml
    │   └── templates/nginx.conf.j2
    ├── mysql/
    │   ├── tasks/main.yml
    │   └── vars/main.yml
    ├── php/
    │   └── tasks/main.yml
    └── wordpress/
        ├── tasks/main.yml
        ├── vars/main.yml
        └── templates/wp-config.php.j2
```

---

## Prerequisites

- Ubuntu/Debian Linux (or WSL on Windows)
- Ansible installed (`sudo apt install ansible -y`)
- SSH access to target host (localhost or remote server)
- Python3 and `python3-pymysql` for MySQL modules

---

## Quick Start

**1. Clone the repository**
```bash
git clone https://github.com/YOUR_USERNAME/ansible-lemp.git
cd ansible-lemp
```

**2. Update the inventory**

Edit `inventory.ini` to point to your target server:
```ini
[webservers]
localhost ansible_connection=ssh ansible_user=YOUR_USERNAME
# OR for a remote server:
# 192.168.1.100 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_ed25519
```

**3. Run the playbook**
```bash
ansible-playbook site.yml --ask-become-pass
```

**4. Access WordPress**

Open your browser and navigate to:
```
http://localhost        → WordPress setup wizard
http://localhost/wp-admin → Admin login
```

---

## Key Ansible Concepts Used

| Concept | Where Used |
|---|---|
| **Roles** | Modular structure — nginx, mysql, php, wordpress |
| **Jinja2 Templates** | `nginx.conf.j2`, `wp-config.php.j2` |
| **Handlers** | Nginx restarts only when config changes |
| **Variables** | `vars/main.yml` per role |
| **Idempotency** | Safe to re-run — won't break existing setup |
| **Loops** | Service status verification across all services |
| **Debug module** | Deployment summary printed at end of run |

---

## Sample Output

```
TASK [Show stack summary]
ok: [localhost] => {
    "msg": [
        "==============================",
        " LEMP + WordPress Deployment  ",
        "==============================",
        "Nginx  : active and running",
        "MySQL  : active and running",
        "PHP    : PHP 8.1.2 (cli)",
        "WordPress URL: http://localhost",
        "WP Admin     : http://localhost/wp-admin",
        "=============================="
    ]
}
```

---

## Managing Services

**Stop the stack:**
```bash
sudo service nginx stop
sudo service php8.1-fpm stop
sudo service mysql stop
```

**Start the stack:**
```bash
sudo service nginx start
sudo service php8.1-fpm start
sudo service mysql start
```

**Or use Ansible:**
```bash
ansible all -b -m service -a "name=nginx state=started"
```

---

## Tested On

- Ubuntu 22.04 LTS (WSL2 on Windows 11)
- Ansible 2.10+
- PHP 8.1.2
- MySQL 8.0
- Nginx 1.18

---

## Author

**Sreehaas**  
SRMIST, Department of Networking and Communications  
Project built as part of Open Source Automation Using Ansible.
