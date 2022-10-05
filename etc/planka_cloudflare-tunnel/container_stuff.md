# Installation of Planka, LinkIN, Heimdall and LibreSpeed with approved SSL-Certificate by Cloudflare Tunnel 
## Only working with Argo Tunnel!
Befor you can doing this, you need to complete the first Guide: [Main Guid](https://github.com/SirSnolte/Docker/blob/main/README.md) already done? Go a head.

### Planka

First create in your local terminal an secretkey:

```
openssl rand -hex 64
```
Copy that and replace in this Stack:
Also make sure you set your BASE_URL right.
Dont put any port in this Stack, the default service port is 1337

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
    environment:
      - BASE_URL=https://planka.yourdomain.com
      - TRUST_PROXY=1
      - DATABASE_URL=postgresql://postgres@postgres/planka
      - SECRET_KEY=3clb3b8c1c4726542675df29d294af2066aad57f8e7544542ca9ec6ac468bc152fac640caf03d9cc40a76185e4a2d8357c659b6de1871b2303b834b25653375a
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

Now deploy this type of code and go to [Cloudflare Zero Trust](https://dash.teams.cloudflare.com/) and set your Cloudflare.com ARG Tunnel but easy via gui.
Create a Network and add your tunnel in it.
Also make sure to link your container to your new network and link your tunnel via Cloudflare Gui to your local container ip:port. In my case: http://172.20.0.3:1337 linked in Cloudflared.
![alt text](https://github.com/SirSnolte/Docker/blob/main/etc/images/cloudflare.jpg)


Login with: demo@demo.demo:demo

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

### LibreSpeed 

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
    restart: always
```

Go to [Cloudflare Zero Trust](https://dash.teams.cloudflare.com/) and set your Cloudflare.com ARG Tunnel but easy via gui.
Create a Network and add your tunnel in it.
Also make sure to link your container to your new network and link your tunnel via Cloudflare Gui to your local container ip:port. In my case: http://172.20.0.5 linked in Cloudflared.
![alt text](https://github.com/SirSnolte/Docker/blob/main/etc/images/cloudflare.jpg)
-------------------------------------------------------------------------------------------------------------------------------------------------------------------


### LinkIn [Please leave a tip For this awesom dude Rizky](https://github.com/RizkyRajitha/linkin/blob/master/docker-compose.yml)

```
version: "3"

services:
  linkin:
    image: ghcr.io/rizkyrajitha/linkin:latest
    ports:
      - "3000:3000"
    environment:
      DATABASE_URL: "postgres://linkin:linkin123@db:5432/linkin"
      HASHSALT: "1234" # random secret key
      NODE_ENV: "production"
    depends_on:
      - db
      - migrate

  migrate:
    image: ghcr.io/rizkyrajitha/linkin:latest
    command: >
      sh -c "npm run prismamigrateprod
      && npm run seed"
    environment:
      DATABASE_URL: "postgres://linkin:linkin123@db:5432/linkin"
      HASHSALT: "123" # random secret key
      NODE_ENV: "production"
    depends_on:
      - db

  db:
    image: postgres:10-alpine
    ports:
      - "5432:5432"
    environment:
      POSTGRES_DB: "linkin"
      POSTGRES_USER: "linkin"
      POSTGRES_PASSWORD: "linkin123"
    volumes:
      - linkin-data:/var/lib/postgresql/data
volumes:
  linkin-data:
```
Go to [Cloudflare Zero Trust](https://dash.teams.cloudflare.com/) and set your Cloudflare.com ARG Tunnel but easy via gui.
Create a Network and add your tunnel in it.
Also make sure to link your container to your new network and link your tunnel via Cloudflare Gui to your local container ip:port. In my case: http://172.20.0.6 linked in Cloudflared.
![alt text](https://github.com/SirSnolte/Docker/blob/main/etc/images/cloudflare.jpg)
-------------------------------------------------------------------------------------------------------------------------------------------------------------------


### Heimdall

```
---
version: "2.1"
services:
  heimdall:
    image: lscr.io/linuxserver/heimdall:latest
    container_name: heimdall
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
    volumes:
      - /path/to/appdata/config>:/config
    restart: always
```
Go to [Cloudflare Zero Trust](https://dash.teams.cloudflare.com/) and set your Cloudflare.com ARG Tunnel but easy via gui.
Create a Network and add your tunnel in it.
Also make sure to link your container to your new network and link your tunnel via Cloudflare Gui to your local container ip:port. In my case: http://172.20.0.6 linked in Cloudflared.
![alt text](https://github.com/SirSnolte/Docker/blob/main/etc/images/cloudflare.jpg)

-------------------------------------------------------------------------------------------------------------------------------------------------------------------


#### WordPress PHP mysql - Stack


```
version: '2'
services:
  wordpress:
    depends_on:
      - db
    image: wordpress
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306      
      WORDPRESS_DB_USER: admin
      WORDPRESS_DB_PASSWORD: admin
      WORDPRESS_DB_NAME: wordpress
    ports:
      - 8082:80
    networks:
      - myNetwork
  db:
    image: mysql
    restart: always
    volumes:
      - ./database:/var/lib/mysql    
    environment:
      MYSQL_ROOT_PASSWORD: admin
      MYSQL_DATABASE: wordpress
      MYSQL_USER: admin
      MYSQL_PASSWORD: admin
    networks:
      - myNetwork
  phpmyadmin:
    depends_on:
      - db
    image: phpmyadmin
    restart: always
    ports:
      - 8083:80
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: admin
    networks:
      - myNetwork
networks:
  myNetwork:
volumes:
  database:
```
Place this in your Container Consol in .httaccess

```
apt-get update
apt-get install nano
```

```
php_value upload_max_filesize 64M
php_value post_max_size 64M
php_value max_execution_time 300
php_value max_input_time 300
```

 ```
ls -a
 ```
#### Jitsi not working

```
sudo apt-get update
 sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
 ```


https://docs.docker.com/engine/install/debian/

```
apt update ; apt install -y build-essential net-tools curl git software-properties-common neofetch apt-transport-https ca-certificates curl gnupg-agent docker.io docker-compose

```

--------------------------
## make me smile:
<a href='https://ko-fi.com/B0B4CGHUO' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://cdn.ko-fi.com/cdn/kofi4.png?v=3' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>


## Things we use in this guid
- [PlankaGitHub_DockerCompose-yml](https://github.com/plankanban/planka/blob/master/docker-compose.yml)
- [Cloudflare Zero Trust](https://dash.teams.cloudflare.com/) 
- [Cloudflare](https://dash.cloudflare.com/)
- [Cloudflared_DOCKER_HUB](https://hub.docker.com/r/cloudflare/cloudflared)
- [WordPress_php mysql](https://gist.github.com/bradtraversy/faa8de544c62eef3f31de406982f1d42?permalink_comment_id=4148371#gistcomment-4148371)



## Other things to explore
- [Basic Guide](https://github.com/SirSnolte/Docker/blob/main/README.md)
