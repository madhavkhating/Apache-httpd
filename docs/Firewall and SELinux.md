On RHEL 9, Apache HTTPD is controlled by firewalld, so you configure the firewall by adding the http and/or https services to the active zone.

### 1. Basic HTTP/HTTPS setup (default ports)**
Run these as root or with sudo:

**Add HTTP (port 80) and HTTPS (port 443) to the default zone**

$sudo firewall-cmd --permanent --zone=work --add-service=http

$sudo firewall-cmd --permanent --zone=work --add-service=https

**Reload firewall to apply**

$sudo firewall-cmd --reload

**This is the recommended way when Apache listens on standard ports 80/443.**

### 2. If you use a custom port
If you changed Listen in httpd.conf (for example, Listen 8080):

**Open custom port (example: 8080/tcp)**

$sudo firewall-cmd --permanent --zone=work --add-port=8080/tcp

**Reload firewall**

$sudo firewall-cmd --reload

![3](https://github.com/user-attachments/assets/e466295e-fb0a-4ec1-a83e-049bd6dd143e)

**3. Verify the firewall rules**

Check what is allowed:

#List active services in the default zone

$sudo firewall-cmd --zone=work --list-services

#List active ports

$sudo firewall-cmd --zone=work --list-ports

![4](https://github.com/user-attachments/assets/da1b0091-2273-402b-aeda-a1bab7e5cc52)


