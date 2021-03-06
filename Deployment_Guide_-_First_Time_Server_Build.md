# Deployment Guide: First Time Server Build

## Assumptions

We expect the person following these instructions to have Linux System Administration experience.

## Background
The recommended minimum environment for hosting the web application is as follows.
* A Linux server, preferably RHEL6.x/CentOS 6.x (el6) with the standard "Server" packages installed.
* 4 cores, contemporary chipset +2.5GHz.
* 4GB of memory.

These instructions are tailored to and tested on CentOS 6.4. However there should be very little effort required to successfully deploy on other el6 distros.

## Assumed Preparation

You have a _server_ machine ready that:
* is a clean el6 server with the standard "Server" packages.
* has EPEL yum repository is installed.
* has Postfix (or equivalent MTA), or else an SMTP server that will accept connections from this server.
* has selinux disabled, unless you are willing to add all the policy allow rules for the application.
* its firewall and iptables should allow incoming tcp traffic to ssh (22), http (80) and https (443).
* it can install packages through yum
* ensure the vm allows password authentication

If you intend to use [AAF Rapid Connect](https://rapid.aaf.edu.au/registration), we recommend registering your service provider first.

See [Registering with AAF Rapid Connect](AAF_Registration.md)

## Running the complete install script

### Create application user
Create a dc21 user. Make sure you remember the password.
```
root@server $ useradd dc21
root@server $ passwd dc21
```
Give that user sudo powers
```
root@server $ visudo
```
Add this line:
```
dc21      ALL=(ALL)      ALL
```

### Download the setup configuration

On the _server_ machine, you will need to download the setup configuration.

Make sure you are logged in as the dc21 user.

```
dc21@server $ cd $HOME
dc21@server $ wget https://github.com/IntersectAustralia/dc21/blob/master/setup_config?raw=true -O setup_config --no-check-certificate
dc21@server $ vi $HOME/setup_config
```

You will need to modify the setup_config with the appropriate values.

## Running the update

Once you have modified the setup configuration, run the following:

```
cd $HOME
bash <(curl -L https://github.com/IntersectAustralia/dc21/blob/master/setup.sh?raw=true)
```
The setup script uses 'expect' and you may receive a prompt for the 'dc21' user's password to install it. After that, the script is fully automated.

What this script does is:

1. Installs Tesseract (for OCR)
2. Installs Apache
3. Installs OpenSSL
4. Installs Redis
5. Installs Ruby 2.0.0-p481
6. Installs Passenger 4.0.45
7. Installs OAI and Tomcat
8. Installs Postgres
9. Installs CRON job to periodically remove temporary zip files used for downloads.
10. Creates a local deploy setup
11. Overwrites the Apache config in `/etc/httpd/conf.d/rails_dc21app.conf`, `/etc/httpd/conf/httpd.conf`, `/etc/httpd/conf.d/ssl.conf`
12. Runs deploy:safe and updates the server, including running migrations
13. Creates an OpenSSL cert and stores it in `/etc/httpd/ssl/server.crt` and `/etc/httpd/ssl/server.key`

You can read up more of the script [here](https://github.com/IntersectAustralia/dc21/tree/master/vm_setup.sh).

### Known issues
* If Github is having server issues, the deploy:safe method might fail during the upgrade. If this happens, follow [the setup script from line 105 onwards](https://github.com/IntersectAustralia/dc21/tree/2.0.x/vm_setup.sh#L105).
* If there are any other issues during the install, please keep a copy of the console output and contact Intersect Australia.

## Post upgrade instructions

### Using a commercial SSL certificate
Using a self-signed certificate will bring up a warning on browsers. If you have a commercial SSL certificate, you can replace the self-signed certificates at `/etc/httpd/ssl/server.crt` and `/etc/httpd/ssl/server.key` on the _server_ machine.

### Configure RIF-CS Harvest
1. Visit "http://server_url/oai"
2. Go to "Data Provider -> Repository Information and Administration". Log in with the user "oai_admin" and the password you set in previous step. Update the repository information:
![jOAI Setup 1](files/joai%20setup%201.png)
3. You can find the dc21 metadata configuration under "Data Provider -> Metadata Files Configuration". Please confirm that it looks correct and reindex it, by clicking the "Reindex" button under the Action column.

Any other optional settings you may be interested in is documented in detail here: http://www.ands.org.au/resource/joai/joai.html

### Update your logos

To update the logos that are displayed on the server, these files have to be updated

* `dc21app/shared/files/public/favicon.ico`
* `dc21app/shared/files/public/icon_app.png` (60x60)
* `dc21app/shared/files/public/icon_app_small.png` (20x20)

### Customise the application
You will now configure some local settings on your _server_ machine. After deploying the server for the first time, a file was created at `~/dc21app/shared/files/config/dc21app_config.yml` on the _server_ machine. This is where you configure settings specific to your installation.

Log in to your _server_ machine and edit the file `~/dc21app/shared/files/config/dc21app_config.yml`
```
dc21@server $ vi ~/dc21app/shared/files/config/dc21app_config.yml
```

* handle_prefix (prefixes onto the ID for packages, defaults to dc21 if not specified)
* notification_email_sender (The FROM: address in notification emails - e.g. password reset, account approval)
* column_mappings (The 'names' used in column mappings. See http://i.imgur.com/WUdbB.png for example)
* for_codes_lookup_base_url (URL of FOR code Server)
* for_codes_path (Local path for FOR code data)
* file_types (additional file types for selection when uploading - RAW is always included so don't add it here)
* ip_addresses (The white list of IP addresses that are allowed to access the download page of published packages without authentication).

On subsequent deploys, this file will NOT be overwritten if it already exists in the server, so your settings will remain on future re-deploys.

## Check that things work
Check that you can successfully visit the site at the hostname or IP you configured.

If you have problems you can check both the Apache logs and also web application logs in the logs directory under the dc21app directory (e.g. under /home/dc21/dc21app/shared/log).

You can now proceed to run through the [Smoke Tests](Smoke_tests.md)
