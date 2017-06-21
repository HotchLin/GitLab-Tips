# GitLab-Tips
## Index
- [Installation](Installation.md)

## SMTP with SendGrid
```shell
sudo nano /etc/gitlab/gitlab.rb
```
> gitlab_rails['smtp_enable'] = true  
gitlab_rails['smtp_address'] = "smtp.sendgrid.net"  
gitlab_rails['smtp_port'] = 587  
gitlab_rails['smtp_user_name'] = "apikey"  
gitlab_rails['smtp_password'] = "your_apikey"  
gitlab_rails['smtp_domain'] = "smtp.sendgrid.net"  
gitlab_rails['smtp_authentication'] = "login"  
gitlab_rails['smtp_enable_starttls_auto'] = true  
gitlab_rails['smtp_tls'] = false

## Maintenance
### Display Maintenance Page
```shell
# Enable
sudo gitlab-ctl deploy-page up

# Disable
sudo gitlab-ctl deploy-page down
```
### Backup for Omnibus installations
```shell
# default backup path:
# /var/opt/gitlab/backups
sudo gitlab-rake gitlab:backup:create
```
### Restore for Omnibus installations
```shell
sudo cp [EPOCH_YYYY_MM_DD_GitLab version]_gitlab_backup.tar /var/opt/gitlab/backups/

sudo gitlab-ctl stop unicorn
sudo gitlab-ctl stop sidekiq

# Verify
sudo gitlab-ctl status

sudo gitlab-rake gitlab:backup:restore BACKUP=[EPOCH_YYYY_MM_DD_GitLab version]
sudo gitlab-ctl start
sudo gitlab-rake gitlab:check SANITIZE=true
```
