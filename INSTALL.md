# The New E-Mail Protocol (NEMP) - web client software - installation instructions

To download the latest version of NEMP web client, run the following commands:

```console
apt update
apt -y upgrade
apt -y install git
git clone https://github.com/libersoft-org/nemp-client-web.git
cd nemp-client-web
```

Now you need to move the content of the "src" folder to your web root subdirectory.

Here are the examples of how to do it based on different web servers:

### NEMP Server

This is the example for NEMP server stored in /root/nemp-server/:

- Move the content of the "src" folder to your web root:

```console
mkdir /root/nemp-server/www/webmail
mv ./src /root/nemp-server/www/webmail
```

- Then open the web browser and navigate to: https://nemp.domain.tld/webmail/

### NGINX

This is the example for web client stored in **/var/www/nemp.domain.tld/webmail/** directory:

- Create the virtual web configuration file for your NGINX server (by default in: /etc/nginx/sites-available/) named by your domain name (for example: **nemp.domain.tld.conf**) and insert this content:

```ini
server {
 listen 80;
 listen [::]:80;
 listen 443 ssl http2;
 listen [::]:443 ssl http2;
 server_name nemp.domain.tld;
 index index.html index.htm;
 root /var/www/nemp.domain.tld/webmail/;
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

Replace **nemp.domain.tld** with your actual domain name and save the file.

... then move the content of the "src" directory to your web server root, for example:

```console
mkdir /var/www/nemp.domain.tld
mkdir /var/www/nemp.domain.tld/webmail
mv ./src /var/www/nemp.domain.tld/webmail/
```

Then restart your NGINX server using:

```console
/etc/init.d/nginx restart
```

**IMPORTANT NOTE:** In this case, you cannot run the NEMP server on the same network port as your NGINX server. Please edit your **settings.json** file and change the HTTPS port to something else than 443 and and turn off the HTTP (by default on port 80) by setting the "http_run" property from "true" to "false" as it is used by NGINX too. Don't forget to specify the NEMP Server HTTPS port in the TXT record in your DNS as well.