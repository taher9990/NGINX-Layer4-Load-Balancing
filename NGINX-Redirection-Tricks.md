
### 1# Redirect to another domain
```
server {
  server_name .mydomain.com;
  return 301 http://www.adifferentdomain.com$request_uri;
}
```