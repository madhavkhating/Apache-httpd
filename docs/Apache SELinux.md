### How to configure SELinux for Apache on RHEL 9
On RHEL 9, Apache HTTPD runs under SELinux in the targeted policy, and the key is to ensure that Apache’s file contexts and allowed operations match the standard policy. Out of the box, SELinux permits httpd to serve files from /var/www/html and listen on standard HTTP/HTTPS ports; any custom path or behavior needs explicit SELinux configuration.

**1. Default SELinux setup for Apache**

By default on RHEL 9:
* Apache runs as domain httpd_t.
* Its content is expected under /var/www/html with contexts like httpd_sys_content_t (static content), httpd_sys_rw_content_t (read/write), etc
* SELinux already allows httpd_t to listen on ports 80/443 and access /var/www/… paths, so no extra SELinux work is needed if you keep that default layout.
  
To verify:
```bash
# Check process context
$sudo ps auxZ | grep httpd

# Check default document root context
$sudo ls -Zd /var/www/html
```
You should see httpd_sys_content_t‑flavored contexts for /var/www/html and descendants.

![5](https://github.com/user-attachments/assets/dcdaeb69-c7a4-4196-a4a7-0b8da476356f)

**2. Configure SELinux for custom document root**

If you change DocumentRoot (for example, to /data/website), you must relabel the new tree:

```bash
$sudo mkdir -p /data/website
$ls -ld /data/website
$ls -ldZ /data/website
# Define SELinux context for custom content (read‑only)
$sudo semanage fcontext -a -t httpd_sys_content_t '/data/website(/.*)?'

# Apply context recursively
$sudo restorecon -Rv /data/website

# Verify
$ls -ldZ /data/website
```

![6](https://github.com/user-attachments/assets/bbb94fba-fc79-4631-a4f1-3b92354f37eb)

**3. SELinux booleans for Apache**

Common useful booleans for Apache on RHEL 9:
you use the setsebool command with the -P flag so the change persists across reboots.

```bash
# Allow Apache to make outgoing network connections (e.g., reverse proxy, APIs)
$sudo setsebool -P httpd_can_network_connect on

# Allow Apache to send mail (e.g., via PHP’s mail() and local MTA)
$sudo setsebool -P httpd_can_sendmail on

# Allow Apache to serve content from user home directories
$sudo setsebool -P httpd_enable_homedirs on

# Unify CGI and server content policy (often needed for modern apps)
$sudo setsebool -P httpd_unified on
```bash

After flipping booleans, reload Apache:

```bash
$sudo systemctl reload httpd
````

**To list all httpd‑related SELinux booleans on RHEL 9, filter the full boolean list with grep httpd.**
```bash
$sudo getsebool -a | grep httpd
```
![7](https://github.com/user-attachments/assets/087652df-0c46-4800-af1b-dfcd172f219b)




