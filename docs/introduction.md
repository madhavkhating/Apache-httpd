## Introduction to Apache httpd web server
Apache HTTP Server (Apache httpd) on Red Hat Enterprise Linux (RHEL) is a free, open‑source web server that listens for HTTP/HTTPS requests and serves static web content or dynamic applications. It is the standard web server package on RHEL, delivered as the httpd RPM, and integrates tightly with RHEL features such as firewalld, SELinux, and systemd.

On RHEL, Apache runs as the httpd service and:
* Listens by default on TCP port 80 for HTTP and 443 for HTTPS, handling client requests from browsers and APIs.
* Serves files from a document root (typically /var/www/html) but can be configured to serve multiple virtual hosts (websites) on the same RHEL server.
* Uses modules such as mod_ssl, mod_rewrite, and mod_proxy to support TLS, URL rewriting, and reverse‑proxy use cases.

**Key configuration locations on RHEL9**

The main configuration files on RHEL httpd are under /etc/httpd:
* Main config:
  * /etc/httpd/conf/httpd.conf contains global directives such as Listen, DocumentRoot, ServerName, and Include statements.
This configuration serves out the contents of /var/www/html for requests coming in to any hostname over plain http.
* Extra configs:
  * /etc/httpd/conf.d/ – where you add extra configuration files (SSL, virtual hosts, etc.).
  * /etc/httpd/conf.modules.d/ – module‑loading files that tie into RHEL’s modular httpd packages.
  
**Key log locations on RHEL 9**
On RHEL 9, Apache HTTP Server (httpd) stores its main log files under /var/log/httpd/ by default, following the standard Red Hat layout. The exact paths assume you are using the stock RHEL‑provided httpd package and have not customized the ErrorLog or CustomLog directives.

**Default RHEL 9 log file locations**
  * Error log (main):
        /var/log/httpd/error_log   :   This is where Apache writes startup errors, configuration‑related issues, and per‑request errors (5xx, SSL, module failures, etc.).
    
  * Access log (main):
        /var/log/httpd/access_log  :  This records HTTP requests (client IP, method, URL, status code, bytes, etc.), typically using the combined or common format.
## Starting Apache httpd on RHEL9
To start Apache HTTPD (httpd) on RHEL 9, you use the standard systemd service commands. By default Apache listens on port 80, and the service is managed as httpd.
Basic start and enable commands, Run these as root or with sudo:


#Install httpd (if not already installed)

$sudo dnf install httpd -y

#Start Apache now

$sudo systemctl start httpd

#Enable it to start automatically at boot

$sudo systemctl enable httpd

#Check httpd status with 

$sudo systemctl status -l httpd.service

![1](https://github.com/user-attachments/assets/18c12f2f-c64c-440b-b425-5f7bc0a45e95)

#Confirm it is listening on port 80
$ss -tlnp | grep :80

#Quick test from localhost

$curl -I http://localhost

![2](https://github.com/user-attachments/assets/b8dc6594-1443-4c6c-a978-261aede086ac)


