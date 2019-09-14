Logs
#Max requests per IP
cat /var/log/nginx/access.log|cut -f 1 -d ' '|sort|uniq -c|sort -nr|less 

###MySQL
#ADD GRANTS
GRANT ALL PRIVILEGES ON database.* TO user@`localhost` IDENTIFIED BY 'password';
#MAKE DUMP dbname to dbname.sql
mysqldump --routines --events --lock-tables dbname > dbname.sql

###SNIPETS
#nginx limits
http {
geo $limit {
 default 1;
8.8.8.8 0;
1.1.1.1 0;
78.140.128.228 0;
}
}

vhost.conf

map $limit $limit_ips {
 0 '';
 1 $binary_remote_addr;
}

limit_req_zone $limit_ips zone=peraddr:10m rate=100r/m;

###nc
#push file fix.sh to host 192.168.1.199 port 60000
cat acme.sh | nc -l 60000 < fix.sh #source
nc 192.168.1.199 60000 | cat > fix.sh #target
