## DNS 
    sudo dnf install bind bind-utils

    /etc/named.conf
    listen-on port 53 {any;}
    allow-query {any;};
    recursion yes;

    systemctl start named.service
    systemctl enable named.service --permanent
    firewall-cmd -add-service=dns

    dig @127.0.0.1 google.com (caching)

## Maintain a DNS zone
    bind can group dns

    1. /etc/named.conf
    zone "example.com" IN {
        type master;
        file "example.com.zone";
    }

    1. /var/named/
        TTL -   Time To Live (life of cache)
        example.com.    A   
        SOA -   State Of Authority
        IN  -   InterNet

        $TTL 1H
        @       IN  SOA @ administrator.example.com. (
                            0       ;   serial
                            1D      ;   refresh
                            1H      ;   retry
                            1w      ;   expire
                            3H )    ;   minimum
        @               NS      ns1.example.com.
        @               NS      ns2.example.com.
        ns1             A       10.11.12.9
        ns2             A       10.11.12.10
        @               A       203.0.113.15
        www             A       203.0.113.15    (optional)
        www             CNAME   203.0.113.15
        example.com.    MX  10  mail.example.com.
                        MX  20  mail2.example.com
        mail            A       203.0.113.80
        mail2           A       203.0.113.81
        server1         AAAA    2001:DB8:10::1,
        example.com.    TXT     "we can write anything in here!"

    2.  sudo systemctl  restart named.service
    3.  dig @localhost example.com
    4.  dig @localhost mail.example.com
    5.  dig @localhost example.com  any


## Configure email aliases
    
    /var/spool/mail/aaron

    sudo dnf install postfix
    sudo systemctl start postfix
    sudo systemctl enable postfix
    send aaron@localhost    <<< "Hello, I'm just testing email"
    cat /var/spool/mail/aaron

    sudo vim /etc/aliases
    sudo newaliases
    send advertising@localhost    <<< "Hello, I'm just testing email"

    sudo vim /etc/aliases
        contact: aaron,john,jane
        /var/spool/mail/aaron
        /var/spool/mail/john
        /var/spool/mail/jane
        advertising: aaron
        advertising: aaron@somewebsite.com
    sudo newaliases
    
## Configure an IMAP and IMAPS service

    Internet Message Access Protocol
    IMAPS - SSL - secured - encrypted

    sudo dnf install dovecot
    sudo systemctl start dovecot
    sudo systemctl enable dovecot
    sudo firewall-cmd -add-service=imap
    sudo firewall-cmd -add-service=imaps
    sudo firewall-cmd --runtime-to-permanent

    /etc/dovecot/dovecot.conf
        protocols = imap  (will enable both imap and imaps)
        listen = 10.11.12.9

    ls /etc/dovecot/conf.d/
    /etc/dovecot/conf.d/10-master.conf
        inet_listern imaps {
            port = 993
        }
    
    /etc/dovecot/conf.d/10-mail.conf
    mail_location = mbox:~/mail:INBOX=/var/mail/%u

    /etc/dovecot/conf.d/10-ssl.conf
    ssl = yes (in real scenario - required)
    ssl_cert
    ssl_key

    
## Configure SSH servers and clients

    vim /etc/ssh/sshd_config

    Port 988
    AddressFamily (any, inet, inet6)
    ListernAddress 10.11.12.9
    PermitRootLogin no
    PasswordAuthentication no
    Match User aaron
        PasswordAuthentication yes
    X11Forwarding yes (gloable)

    sudo systemctl reload sshd.service

    ls -a
        .ssh


    vim .ssh/config
    Host centos
        HostName 10.11.12.9
        Port 22
        User aaron
    chmod 600 .ssh/config
    ssh centos

    <!-- * to create a ssh auth  -->
    ssh-keygen
    cd .ssh
    ls
    id_rsa  id_rsa.pub

    <!-- * to copy .pub file to client  -->
    ssh-copy-id aaron@10.11.12.9

    .ssh/authorized_keys
    chmod 600 authorized_keys

    known_hosts (stores the fingerprint)
    ssh-keygen -R 10.11.12.9    (removes old fingerprints)

    sudo vim /etc/ssh/ssh_config (deafult looks for the Port 22)
    <!-- * it's not adviced to change the port name there in above location. always add a diff config file as below  -->
    vi /etc/ssh/ssh_config.d/99_our-settings.conf
    Port 229

## Restrict access to the HTTP proxy server

    sudo dnf install squid
    sudo systemctl start squid
    sudo systemctl enable squid
    sudo firewall-cmd --add-service=squid --permanent

    sudo vi /etc/squid/squid.conf
        acl localnet src 10.0.0.0/8 (allow only these local networks)
        acl external src 203.0.113.0/24 (allow only these external networks)
        acl SSL_ports port 443 (based on destination ports)
        http_access deny !Safe_ports (don't allow http wihich are not in safeport section)
        http_access deny to_localhost
        http_access allow external

    example to block youtube
        acl youtube dstdomain .youtube.com
        http_access deny youtube

    
    prioty is based on top line in the file

    sudo systemctl reload squid.service (adviced)
    sudo systemctl restart squid.service

## Configure an HTTP server

    sudo dnf install httpd
    sudo firewall-cmd -add-service=http
    sudo firewall-cmd -add-service=https
    sudo firewall-cmd --runtime-to-permanent

    sudo dnf install mod_ssl
    sudo systemctl start httpd
    sudo systemctl enable httpd

    man httpd.conf
    ls /etc/httpd
    ls /etc/httpd/conf.d
    vi /etc/httpd/conf/httpd.conf    (primary config file)
        Listen 10.11.12.9:8080
        ServerAdmin webserver@example.com
        ServerName www.example.com:80 or ServerName 10.11.12.9
        DocumentRoot "/var/www/html"

    sudo vi /etc/httpd/conf.d/two-websites.conf
        <VirtualHost *:80>
            ServerName store.example.com
            DocumentRoot /var/www/store/
        </VirtualHost>

        <VirtualHost *:80>    
            ServerName blog.example.com
            DocumentRoot /var/www/blog/            
        </VirtualHost>

        <VirtualHost 10.11.12.9:80>
        <VirtualHost 203.0.113.5:80>

    apachectl configtest   (warnings should pop up if index.hmtl page are not found in  /var/www location)

    update all the html pages
    /var/www/html/index.html
    /var/www/blog/
    sudo systemctl reload httpd.service

    sudo vim /etc/httpd/config.d/ssl.conf
        <VirtualHost *:443>
            ServerName www.example.com
            SSLEngine on
            SSLCertificateFile "/path/to/file.cert"
            SSLCertificateFile '/path/to/file.key'
        </VirtualHost>

    /etc/httpd/conf.modules.d/
        00-mpm.conf
            #LoadModule mpm_event_module module/mod_mpm_event.so
            #LoadModule mpm_prefork_module module/mod_mpm_ prefork.so
        00-optional.conf (additional modules)
        00-base.conf (basic enabled modules)
    
## Configure HTTP server log files
    ServerRoot  "/etc/httpd"
    ErrorLog    "/var/log/httpd/errors2.log"
    LogLevel    warn
    LogFormat
   
    sudo vi /etc/httpd/conf.d/two-websites.conf
        <VirtualHost *:80>
            ServerName store.example.com
            DocumentRoot /var/www/store/
            CustomLog /var/log/httpd/store.example.com_access.log combained
            ErrorLog /var/log/httpd/store.example.com_access.log
        </VirtualHost>
    sudo systemctl reload httpd.service
    sudo ls /var/log/httpd/

## Restrict access to a web page
    sudo mv /etc/httpd/conf.d/two-websites.conf to /etc/httpd/conf.d/two-websites.conf.disabled
    sudo systemctl reload httpd.service

    sudo vi /etc/httpd/conf/httpd.conf
    DocumentRoot
    <Directory "/var/www/html">
    Options Indexes FollowSymLinks (remove Indexes FollowSymLinks)
    AllowOverride None (All)
    Require all granted
    </Directory>
    <Directory "/var/www/html/admin">
        Require all denied
    </Directory>
    or
    <Directory "/var/www/html/admin">
        Require ip 192.163.1 203.0.0.113 (192.163.1.0/8)
    </Directory>

    <File ".ht*">
        Require all denied
    </File>
    <File "*.txt*">
        Require all denied
    </File>
    
    sudo systemctl reload httpd.service

    sudo htpasswd -c /etc/httpd/passwords aaron
    sudo cat /etc/httpd/passwords
    -D (delete password)


    <Directory "/var/www/html/admin">
        AuthType Basic
        AuthBasicProvider file
        AuthName "Secret admin page"
        AuthUserFile /etc/httpd/passwords
        Require valid-user
    </Directory>


 










































































































































































