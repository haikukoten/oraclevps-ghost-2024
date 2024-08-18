
## Ghost Blog on Oracle VPS (Lifetime Free Tier)(Ghost, Docker, mysql and Caddy)

This project simplifies the setup of a Ghost blogging platform on Oracle Linux 9 using Docker Compose. It leverages the lifetime free **VM.Standard.A1.Flex** server stack for cost efficiency. 
- **Server:** VM.Standard.A1.Flex, 2 CPUs, 6 GB RAM

### Features

-   **Ghost:** The core blogging platform.
-   **MySQL:** Database for Ghost data storage.
-   **Caddy:** Web server with automatic HTTPS.

### Step 1 - Install Docker Engine
-   **Oracle Linux 9:** Ensure your server is running Oracle Linux 9.
-   **Docker:** Install the latest Docker version compatible with RHEL. Follow the official instructions:[https://docs.docker.com/engine/install/rhel/](https://docs.docker.com/engine/install/rhel/)

### Step 2 - Create folder ghost

1.  **Create project directory:**
```bash
mkdir ghost
cd ghost
```
2.  **Create  `docker-compose.yml`:**
```bash
services:
  ghost:
    image: ghost:latest
    environment:
      - url=https://yourdomain.com  # Replace with your actual domain
      - database__client=mysql
      - database__connection__host=db
      - database__connection__user=ghost
      - database__connection__password=ghostpassword
      - database__connection__database=ghost
    volumes:
      - ./content:/var/lib/ghost/content
    depends_on:
      - db

  db:
    image: mysql:8.4.0
    environment:
      - MYSQL_ROOT_PASSWORD=rootpassword
      - MYSQL_USER=ghost
      - MYSQL_PASSWORD=ghostpassword
      - MYSQL_DATABASE=ghost
    volumes:
      - ./mysql-data:/var/lib/mysql

  caddy:
    image: caddy:2
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - caddy_data:/data
      - caddy_config:/config
    depends_on:
      - ghost

volumes:
  caddy_data:
  caddy_config:
```

Change the passwords
    
3.  **Create data directory:**    
```bash
mkdir mysql-data  
```
4.  **Create  `Caddyfile`:**
```bash
nano Caddyfile
```
    
Paste the provided `Caddyfile` content, replacing `yourdomain.com` with your actual domain name.
    
5.  **Start services:**
```bash
docker compose up -d    
```
## Incase website is down
If the site is down, its most possibly due to Ghost CMS
```bash
docker compose ps -a
```
Check for the name of the image <b>ghost:latest</b>
```bash
docker start ghost-ghost-1
```
(whatever the name of that ghost image)

Check the logs for any errors
```bash
docker logs -f ghost-ghost-1
```

### Troubleshooting

-   **Check service status:**
```bash
docker compose ps -a    
```
-   **Restart Ghost if needed:**
    1.  Find the Ghost container name:
        ```bash
        docker compose ps -a
        ```
    2.  Start the container:
        ```bash
        docker start <ghost-container-name>
        ```
-   **View Ghost logs:**
    1.  Find the Ghost container name (as above).
    2.  View logs:
        ```
        docker logs -f <ghost-container-name>
        ```
### Important Notes

- **Provide updated version of mysql and Caddy in the Dockerfile**.
-   **Domain:** Replace `yourdomain.com` with your actual domain in both `docker-compose.yml` and `Caddyfile`.
-   **Passwords:** Change the default passwords (`rootpassword`,  `ghostpassword`) for security.
-   **Firewall:** Ensure your server's firewall allows incoming traffic on ports 80 and 443.

### Contributing

Feel free to fork this repository and submit pull requests to improve the setup process or add new features.

### License

This project is licensed under the MIT License. See the `LICENSE` file for details.## Ghost Blog with Docker Compose on Oracle Linux
