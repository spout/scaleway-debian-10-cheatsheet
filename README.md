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

## fail2ban
```bash
apt install fail2ban
nano /etc/fail2ban/jail.conf

destemail = votremail@domain.com
action = %(action_mwl)s

# action_ => simple ban
# action_mw => ban et envoi de mail
# action_mwl => ban, envoi de mail accompagné des logs

service fail2ban restart
```
