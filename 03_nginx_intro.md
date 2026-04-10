# Nginx Intro

The main folder of nginx is found under:
```text
/etc/nginx
```

Check the complete folder structure (incl. subfolders):
```bash
sudo apt install tree
tree /etc/nginx/
```

## nginx.conf
* This is the main configuration file. It includes settings like logging, configuration of worker-processes and includes other configuration files.
* This is the first read configuration file (from top to bottom).
* Files included via `include`, are inserted at the location of the directive.
* The scope of settings can either be: `http`, `server`, `location` (wide to narrow). Some settings are allowed at multiple scope level. A similair pattern applies for sites that can be general (all domains), or more narrow (specific domain). Settings in a narrow scope overwrite settings in a wider scope.
* conflicting settings within the same scope are rejected, sometimes ignored (sanity check on startup). But anyhow those conflicts can be identified via `nginx -t`.

## conf.d
Folder for configuration files. All files ending with `.conf` are automatically loaded by nginx. 
Allows better structure and delegations.

## sites-available
Used for site (domain) specific configuration files (virtual hosting). Configurations in `sites-available` are **ignored** by nginx.

## sites-enabled
Used to enable the sites defined in `sites-available`. It should contain only symlinks.

## modules-available and modules-enabled
Used for nginx modules (extensions).

## Configuration structure
Nginx uses following hierachical structure:

```nginx
# --- main context (global) ---
user nginx;
worker_processes auto;

events {
    # --- event-context ---
    worker_connections 1024;
}

http {
    # --- HTTP-context (Webserver-Settings) ---
    include /etc/nginx/mime.types;
    
    server {
        # --- server-context (Virtual Host / Website) ---
        listen 80;
        server_name example.com;
        root /var/www/example.com;

        location / {
            # --- location-context (URL-Handling) ---
            try_files $uri $uri/ =404;
        }
    }
}
```

# Document-Root
per default the document-root is defined (in nginx.conf) under:

```text
/var/www/html/
```
Best-practice to generate a custom-directory within `/var/www/` per domain. E.g. the website `example.com` would be:

`/var/www/example.com/`

# Logging
default folder for nginx log files is:

```text
/var/log/nginx/
```
