# Installation of Planka, Heimdall and LibreSpeed with approved SSL-Certificate by Cloudflare Tunnel 
Befor you can doing this, you need to complete the first Guide: [Main Guid](https://github.com/SirSnolte/Docker/blob/main/README.md) already done? Go a head.

### Run Planka by Portainer Stack (working without SSL Tunnel):

Go to your Portainer GUI click on Stack:
Like this:
![alt text](https://github.com/SirSnolte/Docker/blob/main/etc/images/portainer_stack.png)

First create in your local terminal an secretkey:

```
openssl rand -hex 64
```
Copy that and replace in this Stack:
Also make sure you set your BASE_URL right.

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

    environment:
      - BASE_URL=http://172.20.0.3
      - TRUST_PROXY=1
      - DATABASE_URL=postgresql://postgres@postgres/planka
      - SECRET_KEY=3cfb3b8c1c472654267bdf29d294af2066aad57f8e7540662ca9ec6ac468bc152fac640caf03d9cc40a76185e4a2d8357c609b6de1871b2303b834b25653375a

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

Now deploy this type of code

Login with: demo@demo.demo:demo


Go to [Cloudflare Zero Trust](https://dash.teams.cloudflare.com/) and set your Cloudflare.com ARG Tunnel but easy via gui.
Create a Network and add your tunnel in it.
Also make sure to link your container to your new network and link your tunnel via Cloudflare Gui to your local container ip:port. In my case: http://172.20.0.3 linked in Cloudflared and Planka Stack.

Http ERROR
Https proxy Tunnel with planka = endless planka loading screen 

reasons:
- secret_key: hex 64 hash?
- allow_proxy: 0/1?
- no cloudflare ssl certificates installed?

more coming soon 
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Run LibreSpeed via Portainer Stack:

```
---
version: "2.1"
services:
  librespeed:
    image: lscr.io/linuxserver/librespeed:latest
    container_name: librespeed
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
      - PASSWORD=PASSWORD
      - CUSTOM_RESULTS=false #optional
      - DB_TYPE=sqlite #optional
      - DB_NAME=DB_NAME #optional
      - DB_HOSTNAME=DB_HOSTNAME #optional
      - DB_USERNAME=DB_USERNAME #optional
      - DB_PASSWORD=DB_PASSWORD #optional
      - DB_PORT=DB_PORT #optional
    volumes:
      - /path/to/appdata/config:/config
    ports:
      - 81:80
    restart: unless-stopped
```

Go to [Cloudflare Zero Trust](https://dash.teams.cloudflare.com/) and set your Cloudflare.com ARG Tunnel but easy via gui.
Create a Network and add your tunnel in it.
Also make sure to link your container to your new network and link your tunnel via Cloudflare Gui to your local container ip:port. In my case: http://172.20.0.3 linked in Cloudflared and Planka Stack.

Http ERROR
Https proxy Tunnel with planka = endless planka loading screen 

reasons:
- secret_key: hex 64 hash?
- allow_proxy: 0/1?
- no cloudflare ssl certificates installed?


-------------------------------------------------------------------------------------------------------------------------------------------------------------------


## make me smile:
<a href='https://ko-fi.com/B0B4CGHUO' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://cdn.ko-fi.com/cdn/kofi4.png?v=3' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>


## Things we use in this guid
- [PlankaGitHub_DockerCompose-yml](https://github.com/plankanban/planka/blob/master/docker-compose.yml)
- [Cloudflare Zero Trust](https://dash.teams.cloudflare.com/) 
- [Cloudflare](https://dash.cloudflare.com/)
- [Cloudflared_DOCKER_HUB](https://hub.docker.com/r/cloudflare/cloudflared)

## Other things to explore
- [Basic Guid](https://github.com/SirSnolte/Docker/blob/main/README.md)
