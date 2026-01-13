# WPScan Lab - Instructor Configuration Notes

This file documents the exact environment setup, configurations, problems encountered, and technical decisions made while implementing the WPScan Docker lab. Ready for inclusion into MkDocs-based documentation.

---

## 🧱 Environment Setup

* **Host OS:** Windows 10
* **Platform:** Docker Desktop
* **Tools:**

  * `docker-compose`
  * `wpscanteam/wpscan` official Docker image
  * WordPress official image
  * MySQL 5.7 image

### Docker Compose File

```yaml
version: '3.3'

services:
  wordpress:
    image: wordpress
    restart: always
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: root
      WORDPRESS_DB_PASSWORD: example
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - wordpress_data:/var/www/html

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: wordpress
    volumes:
      - db_data:/var/lib/mysql

volumes:
  wordpress_data:
  db_data:
```

---

## 🌐 Network Configuration

* Attempted with external network `labnet`, containers were present and properly assigned IPs.
* **Issue:** WPScan failed to reach `wordpress` by container name due to network isolation or DNS resolution issues.
* **Fix:** Used Docker’s special DNS `host.docker.internal:8080` when running WPScan.

---

## 🧪 WPScan Execution

Run WPScan from a new container using the following:

```bash
# Basic scan
sudo docker run -it --rm --network="bridge" \
  wpscanteam/wpscan \
  --url http://host.docker.internal:8080

# Scan with vulnerability database access
sudo docker run -it --rm --network="bridge" \
  wpscanteam/wpscan \
  --url http://host.docker.internal:8080 \
  --api-token <your_api_token>
```

---

## ⚠️ Troubleshooting

| Issue                         | Cause                              | Fix                                            |
| ----------------------------- | ---------------------------------- | ---------------------------------------------- |
| WPScan "site seems down"      | Apache not started or DB not ready | Wait 60–90 seconds or use `docker logs`        |
| Cannot reach container via IP | Docker bridge DNS isolation        | Use `host.docker.internal:8080`                |
| No plugins/themes found       | Clean WP install                   | Pre-install vulnerable themes/plugins manually |

Additional tips:

* Always verify WordPress is fully running by visiting [http://localhost:8080](http://localhost:8080) before scanning.
* WPScan supports `--enumerate vp` for plugin detection.
* Add `--random-user-agent` for realistic scanning simulation.

---

## 📚 Why This Setup Works Well in Docker

* ✅ **Reproducibility:** Same environment can be shared among students.
* 🔄 **Easily resettable:** Containers can be torn down/restarted in seconds.
* 🔒 **Isolated:** No impact to the host system.
* 🛠️ **Portable:** Compose files and images are easily stored or shared.

---

## ✅ Potential Enhancements

* Add health checks and dependencies to `docker-compose` to manage boot order.
* Automate vulnerable plugin/theme installation.
* Include sample `mkdocs.yml` entry to link this `.md` file.

