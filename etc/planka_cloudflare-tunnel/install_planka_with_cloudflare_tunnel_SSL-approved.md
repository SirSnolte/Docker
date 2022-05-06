# Installation of Planka with approved SSL-Certificate by Cloudflare Argo Tunnel 
Befor you can doing this, you need to complete the first Guide: [Main Guid](https://github.com/SirSnolte/Docker/blob/main/README.md) already done? Go a head.

### Run Planka by Portainer Stack:

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


-------------------------------------------------------------------------------------------------------------------------------------------------------------------
### Install via Docker Cloudflare Zero Trust GUI Tunnel (free to use but Credit Card needed)

First of all you need to transfer your domain lighty to Cloudflare.com but only your name.server take effect. 
Create an [Cloudflare.com](https://dash.cloudflare.com/sign-up/teams) account verify your email adress and add your domain there, follow there instructions.
If its ready, go to [Cloudflare Zero Trust](https://dash.teams.cloudflare.com/) and set your Cloudflare.com ARG Tunnel but easy via gui.
![alt text](https://github.com/SirSnolte/Docker/blob/main/etc/images/cloudflare_zerotrust.png)
There we can generate Docker code by creating a new Tunnel. Just copy and run in your Terminal.  
Create a Network and add your tunnel in it.
Also make sure to link your container to this network and your tunnel to your local container ip:port. In my case: http://172.20.0.3 linked in Cloudflared and Planka Stack.

Https proxy Tunnel with planka = endless planka loading screen 

reasons:
- secret_key: hex 64 hash?
- allow_proxy: 0/1?
- no cloudflare ssl certificates installed?

more coming soon 
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
