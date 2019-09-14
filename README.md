# Logs
### Max requests per IP
```cat /var/log/nginx/access.log|cut -f 1 -d ' '|sort|uniq -c|sort -nr|less ```

# MySQL
### ADD GRANTS
```GRANT ALL PRIVILEGES ON database.* TO user@`localhost` IDENTIFIED BY 'password';```
### MAKE DUMP dbname to dbname.sql
```mysqldump --routines --events --lock-tables dbname > dbname.sql```

# SNIPETS
## nginx limits per ip
```nginx http {
...
geo $limit {
 default 1;
192.168.1.1 0;
192.168.1.2 0;
192.168.1.3 0;
}
}

map $limit $limit_ips {
 0 '';
 1 $binary_remote_addr;
}

server {
...
limit_req_zone $limit_ips zone=peraddr:10m rate=100r/m;
...
}
```
## Limit to uri from ip

```nginx http {
..
geo $ip_allow {
192.168.1.1 1;
192.168.1.2 1;
default 0;
}
...
}

server {
...
   set $check '';
  
   if ( $ip_allow = 0  ) {
      set $check "A";
   }

    if ( $request_uri ~* ((asm_admin\.php.*|wp-login\.php.*|xmlrpc\.php.*|admin\/login\/.*|wp-json.*)$)) {
      set $check "${check}B";
   }

   if ( $check = "AB" ) {
      return 403;
      }
...
}
```

### crontab for all users
```for user in $(cut -d':' -f1 /etc/passwd); do crontab -u $user -l; done```
### push file fix.sh to host 192.168.1.199 port 60000
```cat acme.sh | nc -l 60000 < fix.sh ``` on source

```nc 192.168.1.199 60000 | cat > fix.sh #``` on target
