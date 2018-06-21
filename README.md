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
### GitLab Backup
#### Upgrade to new Version
```shell
sudo apt-get update
sudo apt-get upgrade -y
```
#### Enable Maintenance
```shell
sudo gitlab-ctl deploy-page up
```
#### Backup GitLab configuration
```shell
cd
tar -zcvf gitlab.tar.gz /etc/gitlab
```
#### Backup
```shell
sudo gitlab-rake gitlab:backup:create
```
### GitLab Restore
#### Upgrade to new Version
```shell
sudo apt-get update
sudo apt-get upgrade -y
```
```shell
sudo cp [EPOCH_YYYY_MM_DD_version]_gitlab_backup.tar /var/opt/gitlab/backups/
```
#### Stop unicorn & sidekiq services
```shell
sudo gitlab-ctl stop unicorn
sudo gitlab-ctl stop sidekiq
```
#### Verify service status
```shell
sudo gitlab-ctl status
```
#### Restore
```shell
sudo gitlab-rake gitlab:backup:restore BACKUP=[EPOCH_YYYY_MM_DD_version]
```
#### Start service
```shell
sudo gitlab-ctl start
```
#### Check
```shell
sudo gitlab-rake gitlab:check SANITIZE=true
```
#### Disable Maintenance
```shell
sudo gitlab-ctl deploy-page down
```
### Setup Whilelist of Rack Attack
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
