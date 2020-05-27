# NGINX-Layer4-Load-Balancing
### Below is overview on Folder Structure for NGINX for the important folders that we used
##### but there are other defaul folders we did not mention them below
<br />/etc/nginx/<br />
├── conf.d<br />
│   ├── mail-service-VIP.mail<br />
│   ├── http-service-VIP.http<br />
│   ├── WEB-TCP-L4-VIP.stream<br />
│   └── DB-TCP-L4-VIP.stream<br />
├── nginx.conf<br />
├── sites-available<br />
│   ├── default<br />
├── sites-enabled<br />
│   ├── default -> /etc/nginx/sites-available/default<br />
│   ├── mail-service-VIP.mail -> /etc/nginx/conf.d/mail-service-VIP.mail<br />
│   ├── http-service-VIP.http -> /etc/nginx/conf.d/http-service-VIP.http<br />
│   ├── WEB-TCP-L4-VIP.stream -> /etc/nginx/conf.d/WEB-TCP-L4-VIP.stream<br />
│   └── DB-TCP-L4-VIP.stream -> /etc/nginx/conf.d/DB-TCP-L4-VIP.stream<br />
 <br />
 <br />
 <br />
#### So you need to  follow below steps to create new VIP e.g. TCP/UDP VIP <br />
1- Create a new file under /etc/nginx/conf.d/<VIP-Name>.stream<br />
 <br />
  ```touch /etc/nginx/conf.d/DNS-Service-VIP.stream```
 <br />
2- Link this new file to the enabled folder<br />
 <br />
  ```sudo ln -s /etc/nginx/conf.d/DNS-Service-VIP.stream /etc/nginx/sites-enabled/DNS-Service-VIP.stream<br />```
 <br />
3- Configure the new VIP add below text to /etc/nginx/conf.d/DNS-Service-VIP.stream<br />
 <br />
 ```
  upstream DNS-Service {
       hash $remote_addr consistent;  # This is going to remeber client IP and send him to same DNS server that we served first
       server DNS-SRV1.EXAMPLE.COM:53;
       server DNS-SRV2.EXAMPLE.COM:53;
       server DNS-SRV3.EXAMPLE.COM:53;
       server DNS-SRV4.EXAMPLE.COM:53;
       server DNS-SRV5.EXAMPLE.COM:53;
       server DNS-SRV6.EXAMPLE.COM:53;
     }
   server {
        listen 10.10.100.100 udp;
        proxy_pass DNS-Service;
        proxy_bind 10.10.100.100;
        access_log /var/log/nginx/access.log basic;
        error_log /var/log/nginx/error.log debug;
     }
 ```
  <br />
 4- Now save the file<br />
 5- Test the config <br />
  <br />  ```sudo nginx -t```
 6- Restart NGINX<br />
 <br />   ```sudo systemctl restart nginx```
