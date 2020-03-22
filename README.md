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

## Mail
```bash
apt install exim4-config
dpkg-reconfigure exim4-config
```

1. internet site; mail is sent and received directly using SMTP
2. System mail name: ENTER
3. IP-addresses: ENTER
4. Other destinations: ENTER
5. Domains to relay mail for: ENTER
6. Machines to relay mail for: ENTER
7. Keep number of DNS-queries minimal: NO
8. Delivery method: mbox format
9. Split configuration into small files: NO

## DEB.SURY.ORG
https://deb.sury.org/

```bash
apt-get -y install apt-transport-https lsb-release ca-certificates
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
apt-get update
```

## MariaDB
https://www.geek17.com/fr/content/debian-9-stretch-installer-et-configurer-mariadb-65
https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-debian-9

```bash
apt install mariadb-server
mysql_secure_installation
```

```bash
mysql -u root -p
```

```sql
USE mysql;
UPDATE user SET plugin='' WHERE user='root';
FLUSH PRIVILEGES;
EXIT;
```

```bash
mysql -u root -p
```
