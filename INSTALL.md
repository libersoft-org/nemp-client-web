# The New E-Mail Protocol (NEMP) - web client software - installation instructions

## Download

These are download instructions of this software for the different Linux distributions.

### Debian / Ubuntu Linux

```console
apt update
apt -y upgrade
apt -y install git
cd /root/
git clone https://github.com/libersoft-org/nemp-client-web.git
cd nemp-client-web
```

### CentOS / RHEL / Fedora Linux

```console
dnf -y update
dnf -y install git
cd /root/
git clone https://github.com/libersoft-org/nemp-client-web.git
cd nemp-client-web
```

## Installation

Here are examples based on web server software that you're using:

### NEMP Server

- Move the content of the "src" folder to your NEMP Server web root subdirectory (example for server stored in **/root/nemp-server/** directory):

```console
mkdir /root/nemp-server/src/www/client
mv ./src /root/nemp-server/src/www/client
```

- Then open the web browser and navigate to: https://nemp.domain.tld/client/ (replace **nemp.domain.tld** with your actual NEMP Server domain name)

### NGINX

- Move the content of the "src" folder to your NGINX web root subdirectory (example for server stored in **/var/www/nemp.domain.tld/** directory):

```console
mkdir /var/www/nemp.domain.tld/client
mv ./src /var/www/nemp.domain.tld/client
```

- Then open the web browser and navigate to: https://nemp.domain.tld/client/ (replace **nemp.domain.tld** with your actual NEMP Server domain name)

If you don't have your NGINX server block, here is the example of configuration file (by default in: /etc/nginx/sites-enabled/) named by your domain name (for example: **nemp.domain.tld.conf**):

```ini
server {
 listen 80;
 listen [::]:80;
 listen 443 ssl http2;
 listen [::]:443 ssl http2;
 server_name nemp.domain.tld;
 index index.html index.htm;
 root /var/www/nemp.domain.tld/;
 ssl_certificate /etc/letsencrypt/live/nemp.domain.tld/fullchain.pem;
 ssl_certificate_key /etc/letsencrypt/live/nemp.domain.tld/privkey.pem;

 if ($scheme != "https") {
  return 301 https://$host$request_uri;
 }

 location / {
  try_files $uri /index.html;
 }

 location ~ /\.ht {
  deny all;
 }
}
```

(replace **nemp.domain.tld** with your actual domain name and save the file)

Then restart your NGINX server using:

```console
/etc/init.d/nginx restart
```

**IMPORTANT NOTE:** If you are using this software for NEMP Server, you cannot run the NEMP Server on the same network port as your NGINX Server. Please edit your **settings.json** file in your NEMP Server "src" directory and change the HTTPS port to something else than 443 and and turn off the HTTP (by default on port 80) by setting the "http_run" property from "true" to "false" as it is used by NGINX too. Don't forget to specify the NEMP Server HTTPS port in the TXT record in your DNS as well.
