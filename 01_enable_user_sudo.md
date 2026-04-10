# Make User a sudoer
Instructions to add user to sudoers

## login as root
```bash
su root
```
provide roots password

## edit /etc/sudoers
```bash
vim /etc/sudoers
```

add following line:

```text
youruser    ALL=(ALL:ALL) ALL
```

ensure following line is given (otherwise add it):

```text
%sudo   ALL=(ALL:ALL) ALL
```
