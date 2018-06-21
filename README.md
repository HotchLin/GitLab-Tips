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

## Setup Whilelist of Rack Attack
```shell
sudo nano /etc/gitlab/gitlab.rb
```
> ...  
gitlab_rails['rack_attack_git_basic_auth'] = {  
&nbsp; 'enabled' => true,  
&nbsp; 'ip_whitelist' => ["127.0.0.1", ...etc],  
&nbsp; 'maxretry' => 10,  
&nbsp; 'findtime' => 60,  
&nbsp; 'bantime' => 300  
}  
...
```shell
sudo gitlab-ctl reconfigure
```

## Maintenance
### Display Maintenance Page
```shell
# Enable
sudo gitlab-ctl deploy-page up

# Disable
sudo gitlab-ctl deploy-page down
```
### GitLab Backup
```shell
# Enable Maintenance page
sudo gitlab-ctl deploy-page up

# update to new Version (optional)
sudo apt-get update
sudo apt-get upgrade -y

# Backup configuration
cd
sudo tar -cjvf gitlab.tar -C /etc/gitlab .

# Backup
sudo gitlab-rake gitlab:backup:create

# Copy backup file from backup folder
sudo cp /var/opt/gitlab/backups/[EPOCH_YYYY_MM_DD_version]_gitlab_backup.tar .

# Disable Maintenance page
sudo gitlab-ctl deploy-page down
```
### GitLab Restore
```shell
# Enable Maintenance page
sudo gitlab-ctl deploy-page up

# update to new Version (optional)
sudo apt-get update
sudo apt-get upgrade -y

# Restore configuration
cd
sudo tar xjvf gitlab.tar -C /etc/gitlab
sudo gitlab-ctl reconfigure

# Copy backup file to backup folder
sudo cp [EPOCH_YYYY_MM_DD_version]_gitlab_backup.tar /var/opt/gitlab/backups

# Stop unicorn & sidekiq services
sudo gitlab-ctl stop unicorn
sudo gitlab-ctl stop sidekiq

# Verify service status
sudo gitlab-ctl status

# Restore
sudo gitlab-rake gitlab:backup:restore BACKUP=[EPOCH_YYYY_MM_DD_version]

# Start service
sudo gitlab-ctl start

# Check
sudo gitlab-rake gitlab:check SANITIZE=true

# Disable Maintenance
sudo gitlab-ctl deploy-page down
```
