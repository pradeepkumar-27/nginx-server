# Nginx Server installation on RHEL8

To install NGINX on Red Hat Enterprise Linux 8 (RHEL 8), you can follow these steps:

Update the System:
```
sudo dnf update
```
Install the EPEL Repository:
```
sudo dnf install epel-release
```
Install NGINX:
```
sudo dnf install nginx
```
Start and Enable the NGINX Service:
```
sudo systemctl start nginx

sudo systemctl enable nginx
```
This will start the NGINX service and configure it to start automatically on system boot.

Adjust Firewall Rules:
If you have a firewall enabled, you need to allow HTTP (port 80) and HTTPS (port 443) traffic. You can use the following commands to allow traffic through the firewall:
```
sudo firewall-cmd --add-service=http --permanent

sudo firewall-cmd --add-service=https --permanent

sudo firewall-cmd --reload
```
This configures the firewall to allow incoming HTTP and HTTPS traffic.

---

### Verify the Installation :
Open a web browser and enter your server's IP address or domain name. You should see the default NGINX landing page if the installation was successful.

Now NGINX is installed on your RHEL 8 system, and you can start configuring it for your specific needs. The NGINX configuration files are located in the /etc/nginx/ directory, including the main configuration file nginx.conf.

You can further customize NGINX by modifying the configuration files according to your requirements. Remember to restart NGINX using sudo systemctl restart nginx after making any changes to the configuration.

Please note that the exact steps may vary depending on your specific RHEL 8 setup or any custom configurations you might have.

---

### Enable TLS Encryption on Nginx server
Install OpenSSL
```
sudo dnf install openssl
```
Generate self signed certificate
```
openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 \
    -subj "/C=IN/ST=Tami Nadu/L=Ooty \
    /O=DevOps Org/CN=nginx.devops.org" \
    -keyout /etc/nginx/cert.key -out /etc/nginx/cert.crt
```
Edit /etc/nginx/nginx.conf
```
user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;

    server {
        listen 80;
        server_name nginx.devops.org;
        return 301 https://$host$request_uri;
    }

    server {
        listen 443 ssl;
        server_name nginx.devops.org;

        ssl_certificate /etc/nginx/cert.crt;
        ssl_certificate_key /etc/nginx/cert.key;

        location / {
            root /usr/share/nginx/html;
            index index.html;
        }
    }
}
```
Restart nginx service
```
systemctl restart nginx
```
Configure DNS settings to route nginx.devops.org to the nginx server IP