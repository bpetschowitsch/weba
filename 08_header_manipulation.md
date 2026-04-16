# Nginx Header Manipulation
Instructions to manipulate headers in nginx

## Hide Webserver-Details
The simplest (and anyhow recommended) configuration is to remove the webserver details.
This will set the server header to `nginx` only (without any further details).
This is done e.g. in the main configuration: `/etc/nginx/nginx.conf`

```bash
sudo vim /etc/nginx/nginx.conf
```

and uncomment or add following line:

```nginx
http {

        ##
        # Basic Settings
        ##

        [...]
        server_tokens off; # Recommended practice is to turn this off
        [...]
}
```

## Hide/Replace Webserver at all
To remove or add headers at all, the "headers more module" is needed.
On debian/ubuntu the this module is included in the package `nginx-extras`.
To verify if the package is already installed run:

```bash
sudo apt list nginx-extras --installed
```

to install the package run:

```bash
sudo apt install nginx-extras -y
```

to remove the Server header at all, modify the site configuration:

```bash
sudo vim /etc/nginx/sites-available/*site-name*
```

and add the following line to the server section:

```nginx
server {
    [...]
    more_clear_headers Server;
    [...]
}
```

to replace it with a custom value, use following statement:

```nginx
server {
    [...]
    more_set_headers 'Server: Webserver';
    [...]
}
```

## Security Header

### X-Frame-Options
instructs the browser to not show the website in an iframe.
**Note:** The modern way is using `frame-ancestors` within the CSP header.
edit the site configuration:

```nginx
server {
    [...]
    add_header X-Frame-Options DENY always;          # disable being loaded as iframe at all
    add_header X-Frame-Options SAMEORIGIN always;    # only from same domain allowed
    [...]
}
```
