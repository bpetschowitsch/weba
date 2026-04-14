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

### CSP

#### XSS/CSP Demo
Using Demo-App [Addressbook](enable_php.md)

Using Instructions [XSS-Demo](xss_demo.md)

#### Preparing Website for exercise
as simple playground...add following code-block to the index.html (or index.nginx-debian.html) before **`</body>`**

```html
<input id="hello_world" type="button" value="Hello World!" />

<script nonce="nginx_nonce">
document.getElementById("hello_world").addEventListener('click', hello);

function hello() {
        alert("hello world!");
}
</script>
```

and add a style tag (within head-section):

```html
<style nonce="nginx_nonce">
  h1 {color:red;}
</style>
```

#### CSP general

Set the Content Security Policy. Edit the site configuration.

```nginx
server {
    [...]
    add_header Content-Security-Policy "default-src 'self';" always;    # only resources from our webserver are loaded. Inline script/css tags are blocked as well!
    [...]
}
```

afterwards compare the website to before (comment/uncomment the CSP header).

#### CSP nonce
a simple example to generate a dynamic nonce per request...
edit the site configuration.

```nginx
server {
    [...]
    # nonce
    set $nonce $request_id;
    add_header Content-Security-Policy "default-src 'self' 'nonce-$nonce';" always;    # only resources from our webserver are loaded
    sub_filter 'nginx_nonce' "$nonce";
    sub_filter_once off;
    sub_filter_types text/html;
    [...]
}
```

Note: *nginx_nonce* has to match to the value provided in the *script* and *style* tag.

Note2: Test the website again. The style and script tags should now be allowed, as they are whitelisted by using the nonce tag.

#### CSP hash
nonce requires will and option to modify the source code of webapp...
another option is using hash-values in the CSP header.

comment the lines from above to have the style and script stop working again.

get hash-values from the debug-console of the browser and add it to the CSP instruction.
![firefox debug-console](./images/get_csp_hash.png)

or (for external resources) calculate them using:

```bash
curl -s https://*domain*/*path-to-resource* | openssl dgst -sha384 -binary | openssl base64 -A
```

```nginx
server {
    [...]
    # csp hash
    add_header Content-Security-Policy "default-src 'self' 'nonce-$nonce' 'sha256-4qxDpGEJUcxjIP3NOEWlTKBLTDQ5y6fmRuEEO6ZT9Q0=' 'sha256-IKS/bMyKMqxTwEMnsaKruaZIbhySJGL+EebDepTqwUM=';" always;    # only resources from our webserver are loaded
    [...]
}
```

Note: Test the website again. The style and script tags should now be allowed, as they are whitelisted by providing their hash-value.