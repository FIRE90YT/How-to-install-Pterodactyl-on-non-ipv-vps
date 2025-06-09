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
