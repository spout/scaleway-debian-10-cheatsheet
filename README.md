# scaleway-debian-10-cheatsheet

## SSH / PuTTY
https://www.scaleway.com/en/docs/create-and-connect-to-your-server/

## APT
```bash
apt update
apt upgrade
```

## byobu
```bash
apt install byobu
byobu

# Launch auto at login
byobu-enable
```

## Firewall
https://www.digitalocean.com/community/tutorials/how-to-setup-a-firewall-with-ufw-on-an-ubuntu-and-debian-cloud-server

```bash
apt install ufw

nano /etc/default/ufw
IPV6=no

ufw disable
ufw enable

ufw default deny incoming
ufw default allow outgoing

ufw allow ssh
ufw allow http
ufw allow https

ufw show added
ufw enable
ufw status
```
