# hello world
instruction to prepare nginx site configuration

## nginx site configuration

the site configuration is created within `/etc/nginx/sites-available/`

```bash
cd /etc/nginx/sites-available
```

create a new file for the domain...or easier, copy the exisiting one:

```bash
sudo cp default *my_domain*
```

and edit the file:

```bash
sudo vi *my_domain*
```

```nginx
server {
    listen 80;
    listen [::]:80;
    
    server_name *my_domain*;

    root /var/www/*my_domain*/;
    index index.html index.htm index.nginx-debian.html;


    location / {
        try_files $uri $uri/ =404;
    }
}
```


## folder structure

change to directory `/var/www/`:

```bash
cd /var/www/
```

and create a subfolder for the own domain/website:

```bash
sudo mkdir /var/www/*my_domain*
sudo chown www-data:www-data /var/www/*my_domain*
```

## create hello-world index.html

```bash
sudo vi /var/www/*my_domain*/index.html
```

```html
<html>
    <head>
        <title>Welcome!</title>
    </head>
    <body>
        <h1>Hello World!</h1>
    </body>
</html>
```

```bash
sudo chown www-data:www-data index.html
```

## enable, verify and restart

enable the new site:

```bash
sudo ln -s /etc/nginx/sites-available/*my_domain* /etc/nginx/sites-enabled/*my_domain*
```

validate configuration:

```bash
sudo nginx -t
```

restart nginx:

```bash
sudo systemctl restart nginx
```