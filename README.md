# scaleway-debian-10-cheatsheet

## SSH / PuTTY
https://www.scaleway.com/en/docs/create-and-connect-to-your-server/

## Update
```bash
sudo apt update
sudo apt upgrade
```

## byobu
```bash
sudo apt install byobu
byobu

# Launch auto at login
byobu-enable
```

## New user
```bash
adduser spout
usermod -g www-data spout
```

## sudo
```bash
adduser spout sudo
```

## SSH
```bash
sudo nano /etc/ssh/sshd_config
Port 7022

sudo service ssh restart
```

## Firewall
https://www.digitalocean.com/community/tutorials/how-to-setup-a-firewall-with-ufw-on-an-ubuntu-and-debian-cloud-server

```bash
sudo apt install ufw

sudo nano /etc/default/ufw
IPV6=yes

sudo ufw disable
sudo ufw enable

sudo ufw default deny incoming
sudo ufw default allow outgoing

# ufw allow ssh
sudo ufw allow 7022/tcp
sudo ufw allow http

sudo ufw show added
sudo ufw enable
sudo ufw status
```

## fail2ban
```bash
sudo apt install fail2ban
sudo nano /etc/fail2ban/jail.conf

destemail = votremail@domain.com
action = %(action_mwl)s

# action_ => simple ban
# action_mw => ban et envoi de mail
# action_mwl => ban, envoi de mail accompagné des logs

sudo service fail2ban restart
```

## Mail
```bash
sudo apt install exim4-config
sudo dpkg-reconfigure exim4-config
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

https://www.scaleway.com/fr/faq/pourquoi-je-ne-peux-pas-envoyer-de-mail/

```bash
sudo nano /etc/exim4/update-exim4.conf.conf

MAIN_HARDCODE_PRIMARY_HOSTNAME=foo-bar-baz.pub.cloud.scaleway.com

# https://askubuntu.com/a/1180793/1108984
nano /etc/exim4/exim4.conf.template # add "disable_ipv6 = true" in the main conf section
update-exim4.conf
service exim4 restart
```

## DEB.SURY.ORG
https://deb.sury.org/

```bash
sudo apt-get -y install apt-transport-https lsb-release ca-certificates
sudo wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
sudo sh -c 'echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
sudo apt-get update
```

## MariaDB
https://www.geek17.com/fr/content/debian-9-stretch-installer-et-configurer-mariadb-65
https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-debian-9

```bash
sudo apt install mariadb-server
sudo mysql_secure_installation
```

```bash
sudo mysql -u root -p
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

## PHP
```bash
sudo apt install php7.4-fpm php7.4-gd php7.4-mysql php7.4-pgsql php7.4-sqlite3 php7.4-mbstring php7.4-xml php7.4-intl php7.4-curl php7.4-zip php7.4-soap php7.4-redis
```

## nginx
```bash
sudo apt install nginx

sudo nano /etc/nginx/sites-available/default

root /var/www;
index index.php index.html index.htm

# Uncomment location ~\.php$ {
# Uncomment include snippets/fastcgi-php.conf;
# Uncomment fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;

sudo service nginx reload

sudo chown www-data:www-data /var/www
sudo chmod g+w /var/www

# Gzip
sudo nano /etc/nginx/nginx.conf
# Uncomment:
gzip_vary on;
gzip_proxied any;
gzip_comp_level 6;
gzip_buffers 16 8k;
gzip_http_version 1.1;
gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;

sudo nano /etc/nginx/nginx.conf
# Uncomment:
server_tokens off;

sudo service nginx reload
```

## Locales
```bash
sudo dpkg-reconfigure locales

# fr_FR.UTF-8

locale -a
```

## Gettext
```bash
sudo apt install gettext
```

## Redis
```bash
sudo apt install redis-server
```

## ClamAV
```bash
sudo apt install clamav clamav-freshclam
```

## Adminer
```bash
sudo nano /usr/bin/adminer-update

#!/bin/bash
wget -O /var/www/adminer.php https://www.adminer.org/latest.php

sudo chmod +x /usr/bin/adminer-update
sudo adminer-update
```

## CURL
```bash
sudo apt install curl
```

## Git
```bash
sudo apt install git
```

## ZIP
```bash
sudo apt install zip unzip
```

# www-data
```bash
sudo usermod -g www-data spout
sudo chown www-data:www-data /var/www
sudo chmod g+w /var/www
```

## Backup

### Rclone
https://rclone.org/
https://www.scaleway.com/en/docs/how-to-migrate-object-storage-buckets-with-rclone/

```bash
curl https://rclone.org/install.sh | sudo bash
```

```bash
sudo wget https://raw.githubusercontent.com/spout/debian-10-install-cheatsheet/master/backup.php -O /opt/backup.php
sudo nano /opt/backup.php
sudo chmod +x /opt/backup.php
sudo ln -s /opt/backup.php /etc/cron.daily/backup
sudo run-parts --test /etc/cron.daily
```

## Supervisor
```bash
sudo apt install supervisor
```

## HTTPS / Let's encrypt
https://certbot.eff.org/lets-encrypt/debianbuster-other

```bash
sudo ufw allow https

sudo apt-get install certbot

nano /etc/nginx/sites-available/example.com

location ~ /.well-known {
    allow all;
    root /var/www;
}

sudo nginx -t
sudo service nginx reload

sudo certbot certonly --webroot -w /var/www/ -d example.com -d www.example.com --rsa-key-size 4096
# Wildcard
certbot certonly --manual --preferred-challenges dns --register -d example.com -d *.example.com

sudo certbot renew --dry-run

sudo crontab -e
0 */12 * * * certbot renew --quiet --post-hook "service nginx reload"

sudo openssl dhparam -out /etc/ssl/private/dhparams.pem 4096

sudo nano /etc/nginx/nginx.conf

##
# SSL Settings
##

#ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
#ssl_prefer_server_ciphers on;

ssl_ciphers ECDHE-RSA-AES256-GCM-SHA512:DHE-RSA-AES256-GCM-SHA512:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384;
ssl_protocols TLSv1.2 TLSv1.3;
ssl_prefer_server_ciphers on;
ssl_session_cache shared:SSL:10m;
ssl_dhparam /etc/ssl/private/dhparams.pem;
add_header Strict-Transport-Security "max-age=31536000; includeSubdomains; preload";
```
