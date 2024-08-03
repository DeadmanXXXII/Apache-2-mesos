# Apache-2-mesos
Here's the organized list of commands for **Apache HTTP Server**, **Apache2**, and **Apache Mesos** in order:

### **Apache HTTP Server and Apache2 CLI Commands**

#### **1. Basic Commands**

- **Check Apache version:**
  ```bash
  apachectl -v
  apache2 -v
  ```

- **Check Apache configuration syntax:**
  ```bash
  apachectl configtest
  apache2ctl configtest
  ```

- **Start Apache server:**
  ```bash
  apachectl start
  apache2ctl start
  ```

- **Stop Apache server:**
  ```bash
  apachectl stop
  apache2ctl stop
  ```

- **Restart Apache server:**
  ```bash
  apachectl restart
  apache2ctl restart
  ```

- **Reload Apache server (graceful restart):**
  ```bash
  apachectl graceful
  apache2ctl graceful
  ```

- **Check Apache server status:**
  ```bash
  apachectl status
  apache2ctl status
  ```

- **View Apache logs:**
  ```bash
  tail -f /var/log/httpd/access_log
  tail -f /var/log/httpd/error_log
  tail -f /var/log/apache2/access.log
  tail -f /var/log/apache2/error.log
  ```

#### **2. Configuration and Management**

- **View Apache configuration file:**
  ```bash
  cat /etc/httpd/conf/httpd.conf
  cat /etc/apache2/apache2.conf
  ```

- **Edit Apache configuration file:**
  ```bash
  sudo nano /etc/httpd/conf/httpd.conf
  sudo nano /etc/apache2/apache2.conf
  ```

- **Enable/Disable Apache modules:**
  ```bash
  a2enmod MODULE_NAME
  a2dismod MODULE_NAME
  ```

- **Enable/Disable Apache sites:**
  ```bash
  a2ensite SITE_NAME
  a2dissite SITE_NAME
  ```

- **Create a new virtual host:**
  ```bash
  sudo nano /etc/httpd/conf.d/new-site.conf
  sudo nano /etc/apache2/sites-available/new-site.conf
  ```

- **Disable default site:**
  ```bash
  a2dissite 000-default.conf
  ```

#### **3. SSL/TLS Management**

- **Generate a new self-signed SSL certificate:**
  ```bash
  openssl req -new -x509 -days 365 -nodes -out /etc/ssl/certs/mydomain.crt -keyout /etc/ssl/private/mydomain.key
  ```

- **Configure SSL in Apache:**
  ```bash
  sudo nano /etc/httpd/conf.d/ssl.conf
  sudo nano /etc/apache2/sites-available/default-ssl.conf
  ```

#### **4. Advanced Configuration**

- **Enable mod_rewrite (URL rewriting):**
  ```bash
  a2enmod rewrite
  ```
  **Configure in the site configuration file:**
  ```bash
  <Directory /var/www/html>
      Options Indexes FollowSymLinks
      AllowOverride All
  </Directory>
  ```

- **Set up a reverse proxy:**
  ```bash
  a2enmod proxy
  a2enmod proxy_http
  ```
  **Configure in the site configuration file:**
  ```bash
  <VirtualHost *:80>
      ProxyPreserveHost On
      ProxyPass / http://backendserver/
      ProxyPassReverse / http://backendserver/
  </VirtualHost>
  ```

- **Enable and configure `mod_security` (Web Application Firewall):**
  ```bash
  a2enmod security2
  ```
  **Configure in the security configuration file:**
  ```bash
  sudo nano /etc/apache2/mods-enabled/security2.conf
  ```
  Example configuration:
  ```bash
  SecRuleEngine On
  SecRequestBodyAccess On
  SecResponseBodyAccess Off
  ```

- **Set up URL rewriting rules in `.htaccess`:**
  ```bash
  RewriteEngine On
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteRule ^ index.php [L]
  ```

#### **5. Performance Tuning**

- **Adjust `MaxClients` and `MaxRequestWorkers` settings:**
  ```bash
  sudo nano /etc/httpd/conf/httpd.conf
  ```
  Example configuration:
  ```bash
  MaxRequestWorkers 150
  ```

- **Enable Keep-Alive and configure timeout:**
  ```bash
  sudo nano /etc/httpd/conf/httpd.conf
  ```
  Example configuration:
  ```bash
  KeepAlive On
  KeepAliveTimeout 5
  ```

- **Configure MPM settings for high traffic:**
  ```bash
  sudo nano /etc/httpd/conf.modules.d/00-mpm.conf
  ```
  Example configuration for `event` MPM:
  ```bash
  <IfModule mpm_event_module>
      StartServers          5
      MinSpareThreads       25
      MaxSpareThreads       75
      ThreadLimit           64
      ThreadsPerChild       25
      MaxRequestWorkers     400
      MaxConnectionsPerChild  0
  </IfModule>
  ```

#### **6. Security Enhancements**

- **Disable directory listing:**
  ```bash
  <Directory /var/www/html>
      Options -Indexes
  </Directory>
  ```

- **Configure strict Transport Security (HSTS):**
  ```bash
  Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"
  ```

- **Set up SSL/TLS with strong ciphers:**
  ```bash
  SSLCipherSuite HIGH:!aNULL:!MD5
  SSLProtocol all -SSLv2 -SSLv3
  ```

- **Restrict access by IP:**
  ```bash
  <Directory /var/www/html>
      Require ip 192.168.1.0/24
  </Directory>
  ```

#### **7. Log Management**

- **Rotate Apache logs:**
  ```bash
  sudo nano /etc/logrotate.d/apache2
  ```
  Example configuration:
  ```bash
  /var/log/apache2/*.log {
      daily
      missingok
      rotate 14
      compress
      delaycompress
      notifempty
      create 640 root adm
      sharedscripts
      postrotate
          if [ -f /var/run/apache2/apache2.pid ]; then
              /etc/init.d/apache2 reload > /dev/null
          fi
      endscript
  }
  ```

---

### **Apache Mesos CLI Commands**

#### **1. Basic Commands**

- **Check Mesos version:**
  ```bash
  mesos-version
  ```

- **Start Mesos Master:**
  ```bash
  mesos-master --zk=zk://ZK_HOST:PORT/MESOS
  ```

- **Start Mesos Slave/Agent:**
  ```bash
  mesos-agent --master=MASTER_IP:PORT
  ```

- **Stop Mesos Master:**
  ```bash
  kill $(cat /var/run/mesos/mesos-master.pid)
  ```

- **Stop Mesos Slave/Agent:**
  ```bash
  kill $(cat /var/run/mesos/mesos-agent.pid)
  ```

#### **2. Configuration and Management**

- **Configure Mesos to use ZooKeeper for high availability:**
  ```bash
  mesos-master --zk=zk://ZK_HOST:PORT/MESOS --cluster=CLUSTER_NAME --log_dir=/var/log/mesos
  mesos-agent --master=zk://ZK_HOST:PORT/MESOS --log_dir=/var/log/mesos
  ```

- **Set up Mesos with Marathon as a framework:**
  ```bash
  marathon --master=MASTER_IP:PORT --zk=zk://ZK_HOST:PORT/MARATHON
  ```

- **Configure resource isolation (cgroups):**
  ```bash
  mesos-master --cgroups_enable=true --cgroups_cpu_enable=true --cgroups_memory_enable=true
  ```

#### **3. Monitoring and Debugging**

- **Use `mesos-admin` for administrative tasks:**
  ```bash
  mesos-admin --help
  ```

- **Check Mesos metrics and status:**
  ```bash
  curl http://MASTER_IP:PORT/metrics/snapshot
  curl http://MASTER_IP:PORT/v1/state
  ```

- **Inspect running tasks and frameworks:**
  ```bash
  curl http://MASTER_IP:PORT/v1/tasks
  curl http://MASTER_IP:PORT/v1/frameworks
  ```

- **Analyze Mesos logs for troubleshooting:**
  ```bash
  tail -f /var/log/mesos/mesos-master.log
  tail -f /var/log/mesos/mesos-agent.log
  ```

#### **4. Advanced Operations**

- **Run a custom framework or service:**
  ```bash
  ./custom-framework --master=MASTER_IP:PORT
  ```

- **Scale frameworks or services:**
  ```bash
  curl -X POST -H "Content-Type: application/json" -d '{"scale": 5}' http://MASTER_IP:PORT/v1/frameworks/FRAMEWORK_ID/update
  ```

- **Perform a rolling restart of Mesos components:**
  ```bash
  systemctl restart mesos-master
 ```
Certainly! Continuing from where we left off:

### **Apache Mesos CLI Commands (Continued)**

#### **4. Advanced Operations (Continued)**

- **Perform a rolling restart of Mesos components (continued):**
  ```bash
  systemctl restart mesos-agent
  ```

- **Configure resource limits for Mesos containers:**
  ```bash
  mesos-agent --resources=cpus:4;mem:8192
  ```

- **Enable and configure Mesos monitoring and metrics (using Prometheus):**
  ```bash
  mesos-master --metrics_endpoint=http://localhost:5555/metrics
  mesos-agent --metrics_endpoint=http://localhost:5555/metrics
  ```

- **View and configure frameworks and schedulers:**
  ```bash
  curl http://MASTER_IP:PORT/v1/frameworks
  curl http://MASTER_IP:PORT/v1/scheduler
  ```

- **Update Mesos configuration:**
  ```bash
  systemctl reload mesos-master
  systemctl reload mesos-agent
  ```

- **Use `mesos-dns` for service discovery:**
  ```bash
  mesos-dns --zk=zk://ZK_HOST:PORT/MESOS-DNS --listen=0.0.0.0:53
  ```

### **Advanced Apache, Apache2, and Mesos Commands Summary**

#### **1. Apache HTTP Server and Apache2**

- **Security Enhancements:**
  ```bash
  # Disable Directory Listing
  <Directory /var/www/html>
      Options -Indexes
  </Directory>

  # Configure HSTS (HTTP Strict Transport Security)
  Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"

  # Configure SSL/TLS Protocols and Ciphers
  SSLCipherSuite HIGH:!aNULL:!MD5
  SSLProtocol all -SSLv2 -SSLv3
  ```

- **Performance Tuning:**
  ```bash
  # Adjust MPM settings for high traffic
  <IfModule mpm_event_module>
      StartServers          5
      MinSpareThreads       25
      MaxSpareThreads       75
      ThreadLimit           64
      ThreadsPerChild       25
      MaxRequestWorkers     400
      MaxConnectionsPerChild  0
  </IfModule>

  # Configure Keep-Alive settings
  KeepAlive On
  KeepAliveTimeout 5
  ```

- **Advanced Configuration:**
  ```bash
  # Reverse Proxy Configuration
  <VirtualHost *:80>
      ProxyPreserveHost On
      ProxyPass / http://backendserver/
      ProxyPassReverse / http://backendserver/
  </VirtualHost>

  # URL Rewriting Rules in .htaccess
  RewriteEngine On
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteRule ^ index.php [L]
  ```

#### **2. Apache Mesos**

- **Resource Management:**
  ```bash
  # Configure Mesos with resource isolation
  mesos-master --cgroups_enable=true --cgroups_cpu_enable=true --cgroups_memory_enable=true

  # Set resource limits
  mesos-agent --resources=cpus:4;mem:8192
  ```

- **Advanced Operations:**
  ```bash
  # Scale frameworks or services
  curl -X POST -H "Content-Type: application/json" -d '{"scale": 5}' http://MASTER_IP:PORT/v1/frameworks/FRAMEWORK_ID/update

  # Perform rolling restart of Mesos components
  systemctl restart mesos-master
  systemctl restart mesos-agent

  # Update Mesos configuration
  systemctl reload mesos-master
  systemctl reload mesos-agent

  # Use Mesos DNS for service discovery
  mesos-dns --zk=zk://ZK_HOST:PORT/MESOS-DNS --listen=0.0.0.0:53
  ```

### **General Notes**

- **Configuration Files:**
  - Apache: `/etc/httpd/conf/httpd.conf` (CentOS/RHEL), `/etc/apache2/apache2.conf` (Debian/Ubuntu)
  - Mesos: Configuration varies based on the deployment method and tools used, but common files include `/etc/mesos/mesos-master`, `/etc/mesos/mesos-agent`, and service configuration files.

- **Logs and Monitoring:**
  - Apache: Logs are typically located in `/var/log/httpd/` or `/var/log/apache2/`.
  - Mesos: Logs can be found in `/var/log/mesos/` and monitoring can be configured using external tools like Prometheus.
