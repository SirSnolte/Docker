# Guide for: Docker Portainer.io Cloudflare.com Argo Tunnel (SSL + DDOS Porection)
### Basic instructions *Something for Debian*

If you are starting a project from scratch you will need Debian 10 or something like this to get all these stuff working.
For VServer we prefere a fresh clean Debian 10 like in this guide.


## Get started

Always stay up to date:
```
sudo apt-get update
```

Some helpful utilities for your system:
```
sudo apt-get install \
    nano\
    curl \
    lsb-release
```

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
### UncomplicatedFireWall

Maybe you should install and use a firewall to protect some ports:
```
apt update && apt install ufw
```

In my case:
```
ufw default deny
ufw allow 22
ufw allow 80
ufw allow 443
```

Dont forget to enable your Firewall:
```
ufw enable
```
or
- ufw-```status```
- ufw-```disable```
- ufw-```allow``` PORT
- ufw-```deny``` PORT
- ufw-```default deny```
- ufw_```default allow```

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
### Installation Docker

Please make sure you removed your old docker files & stay up to date:
```
apt update && apt upgrade -V && apt dist-upgrade && apt autoremove
```

Now we are ready to go lets install docker.
First of all we need to download docker and then install docker enigne.

Download Docker:
```
sudo curl -fsSL https://get.docker.com -o get-docker.sh | 
```

Run install cmd:
```
sudo sh get-docker.sh
```

Congr. you have now Docker.
The installation will also take some time.

If its ready add docker to root group and restart your system:
```
sudo usermod -aG docker root
```

You can enable the docker service:
```
sudo systemctl enable docker
```
```
sudo systemctl reboot
```

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
### Installation Portainer ```CE/Community Edition``` or ```EE/Business Edition``` 

Choose the one variant, the first one is the basic Opencource Edition.

> #### Add Portainer as an Container to Docker *Community Version (CE)*
>
> Pull your version of Portainer:
> ```
> docker pull portainer/portainer-ce:2.11.1
> ```
>
> Roll out Portainer docker container:
> ```
> docker run -d -p 8000:8000 -p 9443:9000 -p --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:2.11.1
> ```
or

> #### Add Portainer as an Container to Docker *Portainer Business Version (EE)*
> Before you can use the business version of Portainer, you should have a license key. you can get it here: 
> [Get Portainer Business Edition for free](https://www.portainer.io/pricing/take5)
> Pull your version of Portainer:
> ```
> docker pull portainer/portainer-ee:latest
> ```
>
> Roll out Portainer docker container:
> ```
> docker run -d -p 8000:8000 -p 9443:9443 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ee:latest
> ```


Now lean back and wait for some time try to connect to Portainer: https://localhost:9443/ 

If nothing happens:
```
sudo systemctl reboot
```


-------------------------------------------------------------------------------------------------------------------------------------------------------------------
### Cloudflare change your Domain nameserver *free SSL+DDOS-Protection*

First of all you need to transfer your domain lighty to Cloudflare.com but only your name.server take effect. 
Create an [Cloudflare.com](https://dash.cloudflare.com/sign-up/teams) account verify your email adress and add your domain there, follow there instructions.
  

### Docker Cloudflare Zero Trust Tunnel (free to use but credit card needed)
If its ready, go to [Cloudflare Zero Trust](https://dash.teams.cloudflare.com/) and set your Cloudflare.com ARG Tunnel, but easy via gui.
![alt text](https://github.com/SirSnolte/Docker/blob/main/etc/images/cloudflare_zerotrust.png)

For now you cant set an argo tunnel on portainer. 
There we can generate Docker code by creating a new Tunnel. Just copy and run in your Terminal.
It works perfectly fine with Planka: - [Planka Guide](https://github.com/SirSnolte/Docker/blob/main/etc/planka_cloudflare-tunnel/install_planka_with_cloudflare_tunnel_SSL-approved.md)


-------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Usefull commands

```
sudo systemctl reboot
```

## Things we use in this guide
- [Docker](https://www.docker.com/?utm_source=google&utm_medium=cpc&utm_campaign=search_emea_brand&utm_term=docker_exact)
- [Portainer](https://www.portainer.io)
- [Cloudflare](https://dash.cloudflare.com/)
- [Cloudflare Zero Trust](https://dash.teams.cloudflare.com/)
- [Cloudflared_DOCKER_HUB](https://hub.docker.com/r/cloudflare/cloudflared)
  
## Other things to explore
- [Proxy Manager Guide](https://github.com/SirSnolte/Docker/blob/main/etc/nginx-pm_cloudflare-tunnel/install_nginx_manager_with_cloudflare_tunnel_SSL-approved.md)
- [Planka Guide](https://github.com/SirSnolte/Docker/blob/main/etc/planka_cloudflare-tunnel/install_planka_with_cloudflare_tunnel_SSL-approved.md)