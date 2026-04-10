# Nginx Installation on Debian

Installation of Nginx on a **Debian-System**.

## System update
System and package update:

```bash
sudo apt update
sudo apt upgrade -y
```

### Install Nginx

```bash
sudo apt install nginx -y
```

### Verify Nginx status

```bash
sudo systemctl status nginx
```

### Test Nginx
Testing Nginx via browser: replace *server_ip_address* with your servers IP-Address.
You have to see **"Welcome to nginx!"**.

```
http://server_ip_address
```


