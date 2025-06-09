# How-to-install-Pterodactyl-on-non-ipv-vps
```
apt update && apt install curl -y
```
```
bash <(curl https://pterodactyl-installer.se)
```
# PRESS 0
## NOTE-  type in your domain/subdomain, for eg panel.darknodes.site
### no ufw
### HTTPS using Let's Encrypt? type n
### Assume SSL? type y
### agree HTTPS request? type n
```
##############################################################
* Hostname/FQDN: panel.darknodes.site
* Configure Firewall? false
* Configure Let's Encrypt? false
* Assume SSL? true

##############################################################
```
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
# NOW WINGS

Service Type: https
Service URL: localhost:8443
Additional Settings → TLS → enable "No TLS Verify"
Click save hostname
Go to panel.darknodes.site (or whatever your subdomain is) and check if it works.
```
apt update && apt install curl -y
```
```
bash <(curl https://pterodactyl-installer.se)
```
# PRESS 1
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
Now click next, use the same subdomain as earlier (for eg - node.darknodes.site)
Say y to "Unsupported type of virtualization"
n to UFW, DB user, Let's Encrypt
Refer to step 4, but in Cloudflare: tunnel name → edit → public hostnames → add new
Use the node subdomain (for eg node.darknodes.site)
Service URL: localhost:443
it's wings setup time
# NOW 
Login to panel
Admin → Locations → Create one (for me: INDIA1)
Admin → Nodes → Add node (name it whatever)
Daemon Port: 443
SSL: Not Behind Proxy
Use the node subdomain as the FQDN (for me: node.kvm-i7.host)
Copy config token and run the command as usual
## If panel and node are on different hosts:
```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /2.pem -out /1.pem -subj "/CN=localhost"
```
```
sed -i 's|^\(\s*cert:\s*\).*|\1/1.pem|' /etc/pterodactyl/config.yml
```
```
sed -i 's|^\(\s*key:\s*\).*|\1/2.pem|' /etc/pterodactyl/config.yml
```
```
systemctl restart wings
```
## If panel and node are on the same host:
```
sed -i 's|^\(\s*cert:\s*\).*|\1/1.pem|' /etc/pterodactyl/config.yml
```
```
sed -i 's|^\(\s*key:\s*\).*|\1/2.pem|' /etc/pterodactyl/config.yml
```
```
systemctl restart wings
```
```
# Config for Allos
IP: 0.0.0.0
Alias: localhost
Ports: 1025-2069
```
# Tunneling a MC Server
```
nohup ssh -R 0:0.0.0.0:{your-port} serveo.net &
```
```
cat nohup.out
```
# Then connect to your server via
```
serveo.net:{your-port}
    
