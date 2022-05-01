# Installation of Planka with approved SSL-Certificate by Cloudflare Argo Tunnel 
Befor you can doing this, you need to complete the first Guide: [Main Guid](sss) already done? Go a head.

### Run Planka by Portainer Stack:

We need to edit this Stack like:

```
version: '3'

services:
  planka:
    image: ghcr.io/plankanban/planka:latest
    command: >
      bash -c
        "for i in `seq 1 30`; do
          ./start.sh &&
          s=$$? && break || s=$$?;
          echo \"Tried $$i times. Waiting 5 seconds...\";
          sleep 5;
        done; (exit $$s)"
    volumes:
      - user-avatars:/app/public/user-avatars
      - project-background-images:/app/public/project-background-images
      - attachments:/app/private/attachments
    ports:
      - 3000:1337
      - 44350:44350
    environment:
      - BASE_URL=http://172.19.0.3
      - TRUST_PROXY=0
      - DATABASE_URL=postgresql://postgres@postgres/planka
      - SECRET_KEY=e51eb28e32f94589852b104b9223a1a35046cb8a9d0ee6fec967d49068c8c0185512c1e648b9336eb9685fa9779d3afb330a73567beb35a7d401d42e106dc3d3

    depends_on:
      - postgres

  postgres:
    image: postgres:alpine
    restart: unless-stopped
    volumes:
      - db-data:/var/lib/postgresql/data
    environment:
      - POSTGRES_DB=planka
      - POSTGRES_HOST_AUTH_METHOD=trust

volumes:
  user-avatars:
  project-background-images:
  attachments:
  db-data:

```


-------------------------------------------------------------------------------------------------------------------------------------------------------------------
### Install via Docker Cloudflare Zero Trust GUI Tunnel (free to use but Credit Card needed)

First of all you need to transfer your domain lighty to Cloudflare.com but only your name.server take effect. 
Create an [Cloudflare.com](https://dash.cloudflare.com/sign-up/teams) account verify your email adress and add your domain there, follow there instructions.
If its ready, go to [Cloudflare Zero Trust](https://dash.teams.cloudflare.com/) and set your Cloudflare.com ARG Tunnel but easy via gui.
![alt text](https://github.com/SirSnolte/debian_docker_portainer_NGINX_letsencrypt/blob/main/etc/cloudflare_zerotrust.png)
There we can generate Docker code by creating a new Tunnel. Just copy and run in your Terminal.  

 more is coming soon
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

## usefull commands:
```
sudo systemctl reboot
```

## Things we use in this guid
- [PlankaGitHub_DockerCompose-yml](https://github.com/plankanban/planka/blob/master/docker-compose.yml)
- [Cloudflare Zero Trust](https://dash.teams.cloudflare.com/) 
- [Cloudflare](https://dash.cloudflare.com/)
- [Cloudflared_DOCKER_HUB](https://hub.docker.com/r/cloudflare/cloudflared)

## Other things to explore
- [Basic Guid](https://nginxproxymanager.com/setup/#using-mysql-mariadb-database)
- [NGINX Guid](https://nginxproxymanager.com/setup/#using-mysql-mariadb-database)
