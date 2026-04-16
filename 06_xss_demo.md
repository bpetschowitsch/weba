# XSS/CSP Demo

## Environment
Demo App -> [Addressbook](enable_php.md)

## Demo
check that csp is not set in nginx

## Browser
add following name & press "Speichern":

```html
Charlie <div id="out"></div><script>document.onkeypress = function(e) {document.getElementById('out').innerHTML += e.key};</script>
```

and now type on your keyboard...

## CSP
set CSP in nginx config

```nginx
add_header Content-Security-Policy "default-src 'self'; script-src 'self';" always;
```

repeat the Demo.