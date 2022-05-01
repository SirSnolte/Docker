# Installation of NGINX Proxy Manager with approved SSL-Certificate by Cloudflare Argo Tunnel 
Befor you can doing this, you need to complete the first Guide: [Main Guid](https://github.com/SirSnolte/Docker/blob/main/README.md) already done? Go a head.

### Run NGINX PM  by Portainer Stack:

You will install NGNIX Proxy Manager with MariaDB automatically.
Go to your Portainer GUI click on Stack and deploy this type of code:

```
version: "3"
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    ports:
      # These ports are in format <host-port>:<container-port>
      - '80:80' # Public HTTP Port
      - '443:443' # Public HTTPS Port
      - '81:81' # Admin Web Port
      # Add any other Stream port you want to expose
      # - '21:21' # FTP
    environment:
      DB_MYSQL_HOST: "db"
      DB_MYSQL_PORT: 3306
      DB_MYSQL_USER: "npm"
      DB_MYSQL_PASSWORD: "npm"
      DB_MYSQL_NAME: "npm"
      # Uncomment this if IPv6 is not enabled on your host
      # DISABLE_IPV6: 'true'
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
    depends_on:
      - db

  db:
    image: 'jc21/mariadb-aria:latest'
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: 'npm'
      MYSQL_DATABASE: 'npm'
      MYSQL_USER: 'npm'
      MYSQL_PASSWORD: 'npm'
    volumes:
      - ./data/mysql:/var/lib/mysql
```

Like this:
![alt text](https://github.com/SirSnolte/Docker/blob/main/etc/images/portainer_stack.png)
Push deploy.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
### Install via Docker Cloudflare Zero Trust GUI Tunnel (free to use but Credit Card needed)

First of all you need to transfer your domain lighty to Cloudflare.com but only your name.server take effect. 
Create an [Cloudflare.com](https://dash.cloudflare.com/sign-up/teams) account verify your email adress and add your domain there, follow there instructions.
If its ready, go to [Cloudflare Zero Trust](https://dash.teams.cloudflare.com/) and set your Cloudflare.com ARG Tunnel but easy via gui.
![alt text](https://github.com/SirSnolte/Docker/blob/main/etc/images/cloudflare_zerotrust.png)
There we can generate Docker code by creating a new Tunnel. Just copy and run in your Terminal.  

 more is coming soon
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

## usefull commands:
```
sudo systemctl reboot
```


<a href='https://ko-fi.com/B0B4CGHUO' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://cdn.ko-fi.com/cdn/kofi4.png?v=3' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>


## Things we use in this guide
- [NGINX Proxy Manager](https://nginxproxymanager.com/setup/#using-mysql-mariadb-database)
- [Cloudflare Zero Trust](https://dash.teams.cloudflare.com/) 
- [Cloudflare](https://dash.cloudflare.com/)
- [Cloudflared_DOCKER_HUB](https://hub.docker.com/r/cloudflare/cloudflared)

## Other things to explore
- [Basic Guide](https://github.com/SirSnolte/Docker/blob/main/README.md)
- [Planka Guide](https://github.com/SirSnolte/Docker/blob/main/etc/planka_cloudflare-tunnel/install_planka_with_cloudflare_tunnel_SSL-approved.md)

