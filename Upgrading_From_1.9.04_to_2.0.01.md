# Upgrading From 1.9.04 to 2.0.01

There is a significant change to the deployment process for 2.0.01 which no longer requires a _deploy_ machine.
These instructions assume you have deployed DC21 based on the [v1.9.04 Deployment Guide](https://github.com/IntersectAustralia/dc21-doc/blob/1.9.04/Deployment_Guide.md) and have a _server_ machine up and running.

We highly recommend backing up the entire server before proceeding.

## Assumptions

We expect the person following these instructions to have Linux System Administration experience.

## Assumed Preparation

You have a _server_ machine ready with the following environment:
* A Linux server, preferably RHEL6.x/CentOS 6.x (el6) with the standard "Server" packages installed.
* 4 cores, contemporary chipset +2.5GHz.
* 4GB of memory.
* Postgresql (8.1.x or later)
* is a clean el6 server with the standard "Server" packages.
* has EPEL yum repository is installed.
* has Postfix (or equivalent MTA), or else an SMTP server that will accept connections from this server.
* has selinux disabled, unless you are willing to add all the policy allow rules for the application.
* its firewall and iptables should allow incoming tcp traffic to ssh (22), http (80) and https (443).
* can install packages through yum
* has an FQDN
* has a user 'dc21'
* has DC21 v1.9.04 deployed
* has Redis manually installed based on [v1.9.04 Redis Setup](https://github.com/IntersectAustralia/dc21-doc/blob/1.9.04/Setting_up_redis_and_resque.md)
* has OAI set up
* has Postgres set up
* has RVM and Ruby 1.9.2-p290 installed

## Updating the server

### Download the setup configuration

On the _server_ machine, you will need to download the setup configuration.

```
cd $HOME
wget https://github.com/IntersectAustralia/dc21/raw/2.0.01/setup_config
vi $HOME/setup_config

```

You will need to modify the setup_config with the appropriate values.

### Running the update

Once you have modified the setup configuration, run the following:

```
cd $HOME
bash <(curl https://github.com/IntersectAustralia/dc21/raw/2.0.01/setup.sh)
```
The setup script uses 'expect' and you may receive a prompt for the 'dc21' user's password to install it. After that, the script is fully automated.

What this script does is:
1. Installs Tesseract (for OCR)
2. Installs OpenSSL
3. Installs Shibboleth
4. Installs Redis
5. Installs Ruby 1.9.3-p448
6. Installs Passenger 3.0.21
7. Creates a local deploy setup
8. Overwrites the Apache config in `/etc/httpd/conf.d/rails_dc21app.conf`, `/etc/httpd/conf/httpd.conf`, `/etc/httpd/conf.d/ssl.conf`, '/etc/httpd/conf.d/shib.conf'
9. Runs deploy:safe and updates the server, including running migrations
10. Creates an OpenSSL cert and stores it in /etc/httpd/ssl/server.crt and /etc/httpd/ssl/server.key
11. Displays the AAF certificate you have to register with. Please copy this as it will be used later.
12. Overwrite the existing logos with the generic DC21 product logo.

The script will NOT:
* Reinstall jOAI
* Reinstall Postgres
* Overwrite cron settings
* Overwrite /data/dc21app_extra_config.yml

You can read up more of the script [here](https://github.com/IntersectAustralia/dc21/blob/2.0.01/vm_setup.sh).

## Post upgrade instructions

### Register your server with AAF

See [Registering your server with AAF](AAF_Registration.md).

### Update your external README.HTML templates

The default README.HTML templates have been updated to include changes such as labels and parent/child relationships. It is recommended that you update your README.HTML to include these changes.

### Update your logos

To update the logos that are displayed on the server, these files have to be updated

* `dc21app/shared/files/public/favicon.ico`
* `dc21app/shared/files/public/icon_app.png` (60x60)
* `dc21app/shared/files/public/icon_app_small.png` (20x20)

### Transfer changes made from /data/dc21app_extra_config.yml to new shared config location.

The list of shared files are found in `dc21app/shared/files` in the following hierarchy:

```
config/environments/production.rb
config/deploy/production.rb
config/database.yml
config/dc21app_config.yml
config/shibboleth.yml
public/favicon.ico
public/icon_app.png
public/icon_app_small.png
```

Please transfer over any changes to the following configurations in `dc21app_extra_config.yml` into `config/dc21app_config.yml`:
```
readme_template_directory:
readme_template_file:
handle_prefix: (previously known as hiev_handle_prefix)
notification_email_sender:
column_mappings:
for_codes_lookup_base_url:
for_codes_path:
file_types:
ip_addresses:
```

## Check that things work
Check that you can successfully visit the site at the hostname or IP you configured.

If you have problems you can check both the Apache logs and also web application logs in the logs directory under the dc21app directory (e.g. under /home/dc21/dc21app/shared/log).

You can now proceed to run through the [Smoke Tests](Smoke_tests.md)
