# Setup GitLab with Let's Encrypt on Ubuntu 16.04
## Install GitLab
```shell
sudo apt-get update -y
sudo apt-get install -y curl openssh-server ca-certificates postfix
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
sudo apt-get install -y gitlab-ce
```

## Install Certbot, the Let's Encrypt Client
```shell
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update -y
sudo apt-get install -y certbot
sudo mkdir -p /var/www/letsencrypt
sudo nano /etc/gitlab/gitlab.rb
```
> . . .  
external_url 'http://[your_domain]'  
. . .  
nginx['custom_gitlab_server_config'] = "location ^~ /.well-known { root /var/www/letsencrypt; }"  
. . .
```shell
sudo gitlab-ctl reconfigure
sudo certbot certonly --webroot --webroot-path=/var/www/letsencrypt -d [your_domain]
sudo crontab -e
```
> . . .  
15 3 * * * /usr/bin/certbot renew --quiet --renew-hook "/usr/bin/gitlab-ctl restart nginx"

## Edit the GitLab configuration
```shell
sudo nano /etc/gitlab/gitlab.rb
```
> . . .  
external_url 'https://your_domain'  
. . .  
nginx['redirect_http_to_https'] = true  
. . .  
nginx['ssl_certificate'] = "/etc/letsencrypt/live/your_domain/fullchain.pem"  
nginx['ssl_certificate_key'] = "/etc/letsencrypt/live/your_domain/privkey.pem"  
. . .  
```shell
sudo gitlab-ctl reconfigure
```
