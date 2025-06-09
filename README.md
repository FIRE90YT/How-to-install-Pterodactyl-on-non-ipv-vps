# How-to-install-Pterodactyl-on-non-ipv-vps
```
apt update && apt install curl -y
```
```
bash <(curl https://pterodactyl-installer.se)
```
## NOTE-  type in your domain/subdomain, for eg panel.darknodes.eg
### no ufw
### HTTPS using Let's Encrypt? type n
### Assume SSL? type y
### agree HTTPS request? type n
##############################################################
* Hostname/FQDN: panel.darknodes.eg
* Configure Firewall? false
* Configure Let's Encrypt? false
* Assume SSL? true

##############################################################
```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /2.pem -out /1.pem -subj "/CN=localhost"
```
```
sed -i 's|^\s*ssl_certificate\s\+.*|    ssl_certificate /1.pem;|' /etc/nginx/sites-available/pterodactyl.conf
```
```
sed -i 's|^\s*ssl_certificate_key\s\+.*|    ssl_certificate_key /2.pem;|' /etc/nginx/sites-available/pterodactyl.conf
```
```
sed -i 's/\b443\b/8443/g; s/\b80\b/8000/g' /etc/nginx/sites-available/pterodactyl.conf
```
```
systemctl restart nginx
```
# no output is OK
Go to one.dash.cloudflare.com

Network → Tunnels → Create Tunnel
Select cloudflared, name it whatever you want
Choose Debian → click under "If you don’t have cloudflared installed on your machine"
Paste the copied install command into the VPS shell
Then click under "After you have installed cloudflared on your machine, you can install a service to automatically run your tunnel whenever your machine starts"
Again, Paste the copied install command into the VPS shell.
Now click next, use the same subdomain as earlier (for eg - panel.darknodes.site)
Service Type: https
Service URL: localhost:8443
Additional Settings → TLS → enable "No TLS Verify"
Click save hostname
Go to panel.darknodes.site (or whatever your subdomain is) and check if it works.

