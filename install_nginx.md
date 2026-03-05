# Nginx Installation on Debian

Installation of Nginx on a **Debian-System**.

## System update
System and package update:

```bash
sudo apt update
sudo apt upgrade -y
```

<<<<<<< Updated upstream
### Install Nginx
=======
## Nginx installieren
>>>>>>> Stashed changes

```bash
sudo apt install nginx -y
```

<<<<<<< Updated upstream
### Verify Nginx status
=======
## Nginx Status prüfen
>>>>>>> Stashed changes

```bash
sudo systemctl status nginx
```

<<<<<<< Updated upstream
### Test Nginx
Testing Nginx via browser: replace *server_ip_address* with your servers IP-Address.
You have to see **"Welcome to nginx!"**.
=======
## Nginx testen
In den Webbrowser untenstehende URL eingeben (server_ip_address durch eine IP-Adresse ersetzen):
Es sollte **"Welcome to nginx!"** ersichtlich sein.
>>>>>>> Stashed changes

```
http://server_ip_address
```


