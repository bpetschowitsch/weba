# Nginx Installation auf Debian

Installation von Nginx auf einem **Debian-System**.

## System aktualisieren
System und Paketlisten updaten:

```bash
sudo apt update
sudo apt upgrade -y
```

### Nginx installieren
Nginx installiern:

```bash
sudo apt install nginx -y
```

### Nginx Status prüfen
Nginx Status prüfen

```bash
sudo systemctl status nginx
```

### Nginx testen
Nginx per Browser testen:
Es sollte **"Welcome to nginx!"** ersichtlich sein.

```
http://server_ip_address
```

