# Enable PHP in Nginx

run in bash:

```bash
sudo apt install php-fpm -y
```

get php version/sock

```bash
ls -l /run/php/*.sock
```

add to nginx config (server section):

```nginx
# pass PHP scripts to FastCGI server
#
location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.4-fpm.sock; # <-- use version from output above!
}
```

validate nginx configuration & restart if ok:

```bash
sudo nginx -t
sudo systemctl reload nginx
```

create Demo-App:

```bash
sudo vi /var/www/*domain-name*/*webapp*.php
```

## Demo Webapp (Addressbook)

```php
<?php
// Wir simulieren eine einfache Speicherung in einer Textdatei
$db_file = 'names.txt';

// 1. Speichern, wenn das Formular abgeschickt wurde
if (isset($_POST['name'])) {
    file_put_contents($db_file, $_POST['name'] . PHP_EOL, FILE_APPEND);
}

// 2. Auslesen der Namen
$names = file_exists($db_file) ? file($db_file) : [];
?>

<!DOCTYPE html>
<html>
<head>
    <title>Demo: Addressbook</title>
</head>
<body>

    <h2>Einfaches Adressbuch</h2>
    
    <form method="POST">
        <input type="text" name="name" placeholder="Name eingeben..." required>
        <button type="submit">Speichern</button>
    </form>

    <hr>

    <h3>Eintr&auml;ge:</h3>
    <?php foreach ($names as $name): ?>
        <div class="entry">
            Benutzer: <?php echo $name; ?>
        </div>
    <?php endforeach; ?>

    <p><small><a href="?clear=1">Liste leeren</a></small></p>
    <?php if(isset($_GET['clear'])) { file_put_contents($db_file, "" . PHP_EOL); } ?>

</body>
</html>
```

create txt file (as database):

```bash
cd /var/www/*domain-name*
sudo touch names.txt
sudo chown www-data:www-data names.txt
sudo chown www-data:www-data *webapp*
```

testing the webapp:

```
http://*domain-name*/*webapp*.php
```

