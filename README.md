
# Yii2 Advanced App Deployment with Docker Swarm & Ansible

This project provides a fully automated deployment setup for a Yii2 Advanced PHP application using Docker Swarm and Ansible. It includes templated configuration for dynamic environments and clean orchestration of services using `docker stack deploy`.

---

## 📁 Directory Structure

```text
.
├── playbook.yml                        # Main Ansible playbook
└── templates/
    ├── docker-compose.yml.j2          # Docker Swarm-compatible Compose template
    ├── main-local.php.j2              # Yii2 configuration template (e.g. DB credentials)
    └── yii2app.j2                     # Optional systemd or service template (if used)
```

---

## 🧱 System Architecture

- **Yii2 Advanced App** split into `frontend` and `backend`
- **MySQL 5.7** as database
- **Docker Swarm** for container orchestration
- **Ansible** for infrastructure provisioning
- **Dynamic Jinja2 templating** for config generation

---

## 📦 Docker Compose Template (`docker-compose.yml.j2`)

Ensure you are using prebuilt Docker images. Swarm mode **does not support** the `build:` directive.

## ⚙️ Ansible Playbook (`playbook.yml`)

## 🚀 How to Deploy

1. **Build & Push your Docker images** to Docker Hub
2. **Initialize Docker Swarm** (only once):

   ```bash
   docker swarm init
   ```

3. **Run the playbook**:

   ```bash
   ansible-playbook playbook.yml -i localhost,
   ```

4. **Access the services**:

   | Component | URL                |
   |----------|--------------------|
   | Frontend | http://<ip>:20080  |
   | Backend  | http://<ip>:21080  |

---

## 🛑 NGINX Port Conflict

If NGINX fails with:

```
bind() to 0.0.0.0:80 failed (98: Address already in use)
```

### ➤ Fix: Disable or Change NGINX Port

```bash
sudo systemctl stop nginx
sudo systemctl disable nginx
```

Or change port in `/etc/nginx/sites-available/default`:

```nginx
listen 8080;
```

---

## 🧪 Troubleshooting

- Validate Docker Compose:

  ```bash
  docker-compose -f docker-compose.yml config
  ```

- Inspect stack status:

  ```bash
  docker stack services yii2app
  docker service ps yii2app_backend
  ```

- Check NGINX:

  ```bash
  sudo nginx -t
  sudo systemctl status nginx
  sudo journalctl -xeu nginx
  ```

---

## ✅ Requirements

- Ubuntu 20.04+ with root or sudo
- Docker & Docker Compose installed
- Docker Swarm initialized
- Ansible installed
- Prebuilt Docker images pushed to Docker Hub

---

## 📌 Future Improvements

- Add volumes for MySQL persistence
- HTTPS with Let's Encrypt or Traefik
- CI/CD pipeline for image build + deploy
- NGINX load balancing / reverse proxy

---

## 🙌 Author

**Kaif Shakeel**  
Built and deployed using Docker Swarm, Ansible & Yii2  
Customizable via Jinja2 templates

---

> For issues or improvements, feel free to fork and PR.