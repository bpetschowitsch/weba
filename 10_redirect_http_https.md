# Redirect http to https
to allow only encrypted connections, modify nginx site configuration like this:

create a new server section like this:

```nginx
server {
        listen 80;
        listen [::]:80;

        server_name *domain-name*

        return 308 https://$host$request_uri;
}

remove the:
* listen 80;
* listen [::]:80;
statements from the live configuration:

server {
        listen 443 ssl default_server;
        listen [::]:443 ssl default_server;

        server_name *

        [...]
}
```
