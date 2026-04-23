# Let's Encrypt 

retrieving a valid X.509 certificate signed by let's encrypt

## install acme client
First step is to install the acme client:

```bash
sudo apt install certbot python3-certbot-nginx -y
```

## automatic retrieval & manual configuration

### retrieve certificate
Full automatic generation & configuration.
Certbot will generate:
* the keys and CSR
* perform the http-challenge
* retrieve the signed certificate


```bash
sudo certbot certonly --nginx --register-unsafely-without-email -d *domain-name*
```

the certificate can be found in:

```bash
ls /etc/letsencrypt/live/*domain-name*/
```

### nginx config
to configure ssl (tls) add/modify following lines:

```nginx
listen 443 ssl;
ssl_certificate /etc/letsencrypt/live/*domain-name*/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/*domain-name*/privkey.pem;
include /etc/letsencrypt/options-ssl-nginx.conf;
ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;
```

### test by using https
Your website should open now via:

```URL
https://*domain-name*
```

## full automatic

this will additionally configure nginx after successful retrival of the certificate.

ensure the nginx config has the server_name set correctly:

### prepare nginx config
Include the server_name to the nginx site config:

```nginx
listen 80;
server_name *domain-name*
```

### retrieve certificate & let certbot configure nginx

```bash
sudo certbot --nginx --register-unsafely-without-email -d *domain-name*
```

Important: manual check the complete configuration! All site-configurations!