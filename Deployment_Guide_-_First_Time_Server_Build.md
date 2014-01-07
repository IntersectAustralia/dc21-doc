## Assumptions

We expect the person following these instructions to have Linux System Administration experience.

## Background
The recommended minimum environment for hosting the web application is as follows.
* A Linux server, preferably RHEL6.x/CentOS 6.x (el6) with the standard "Server" packages installed.
* 4 cores, contemporary chipset +2.5GHz.
* 4GB of memory.
* Postgresql (8.1.x or later)

These instructions are tailored to and tested on CentOS 6.2. However there should be very little effort required to successfully deploy on other el5/el6 distros.

## Assumed Preparation
Make sure you have read the introductory info explaining the need for a _server_ and a _deploy_ machine.

You have a _server_ machine ready that:
* is a clean el6 server with the standard "Server" packages.
* has EPEL yum repository is installed.
* has Postfix (or equivalent MTA), or else an SMTP server that will accept connections from this server.
* has selinux disabled, unless you are willing to add all the policy allow rules for the application.
* its firewall and iptables should allow incoming tcp traffic to ssh (22), http (80) and optionally https (443 - HTTPS is beyond the scope of this document).
* it can install packages through yum

You have a _deploy_ machine ready - you can deploy from any Unix-based system, e.g. Mac OSX, CentOS, etc. Windows may work too, but we have not tested it. If you want to follow our instructions precisely, you will need a deployment machine that:
* is a clean el6 server with the standard "Server" packages.
* has EPEL yum repository is installed.

Reminder: in the commands below we refer to the machine that runs the deployments as _deploy_ and the machine that runs the web application as _server_.

## Server preparation
This sets up necessary software on your _server_ machine.
### Install packages:
For el6:
```
root@server $ yum install -y gcc gcc-c++ patch readline readline-devel zlib zlib-devel libyaml-devel libffi-devel openssl openssl-devel make bzip2 autoconf automake libtool bison httpd httpd-devel apr-devel apr-util-devel mod_ssl mod_xsendfile curl curl-devel openssl openssl-devel tzdata libxml2 libxml2-devel libxslt libxslt-devel sqlite-devel git postgresql-server postgresql postgresql-devel
```

For el5:
```
root@server $ yum install -y gcc gcc-c++ patch readline readline-devel zlib zlib-devel libyaml-devel libffi-devel openssl openssl-devel make bzip2 autoconf automake libtool bison httpd httpd-devel apr-devel apr-util-devel mod_ssl mod_xsendfile curl curl-devel openssl openssl-devel tzdata libxml2 libxml2-devel libxslt libxslt-devel sqlite-devel git postgresql84-server postgresql84 postgresql84-devel
```

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

### Set up Postgres database and accounts
Make sure you remember the dc21 postgres user password as you'll be needing it later.
```
root@server $ /sbin/service postgresql initdb
root@server $ service postgresql start
root@server $ su - postgres
postgres@server $ createuser -P dc21
=> Enter password for new role: <enter a secure password>
=> Enter it again: <reenter that password>
=> Shall the new role be a superuser? (y/n) y
postgres@server $ createdb dc21
postgres@server $ vi data/pg_hba.conf
```

Edit the last lines so it looks like this:
```
# "local" is for Unix domain socket connections only
local   all         all                               trust
# IPv4 local connections:
host    all         all         127.0.0.1/32          password
# IPv6 local connections:
host    all         all         ::1/128               password
```

As root, restart Postgres and enable services at boot:
```
postgres@server $ exit
root@server # service postgresql restart
Stopping postgresql service:                               [  OK  ]
Starting postgresql service:                               [  OK  ]
root@server # /sbin/chkconfig postgresql on
root@server # /sbin/chkconfig httpd on
```

### Install RVM and Ruby
You should install RVM as the application user. RVM does change the install process from time to time, if you're having problems check: https://rvm.io/rvm/install/

```
root@server $ su - dc21
dc21@server $ curl -L http://get.rvm.io | bash -s stable --ruby=1.9.2-p290
dc21@server $ source ~/.rvm/scripts/rvm
dc21@server $ rvm use 1.9.2-p290
dc21@server $ rvm gemset create dc21app
```

### Set up Cron job to clear temporary zips created by DC21
When downloading from a cart, a temporary zip is created and then served through the XSendfile Apache module. These temporary files should be cleared regularly.

```
root@server $ su - dc21
dc21@server $ crontab -e
```
Add this to the list
```
0 */4 * * * find /tmp/download_zip* -atime +0 -type f -exec rm -f '{}' \;
```

The above deletes the temporary zips created at least 1 day ago, every 4 hours.

For more information on setting up the schedule, see http://www.adminschoice.com/crontab-quick-reference/

## Setting up the deployment machine
The below sets up necessary software on your _deploy_ machine.
### Installing packages
As root, install the following packages:
```
root@deploy $ yum install -y gcc gcc-c++ patch readline readline-devel zlib zlib-devel libyaml-devel libffi-devel openssl openssl-devel make bzip2 autoconf automake libtool bison httpd httpd-devel apr-devel apr-util-devel mod_ssl mod_xsendfile curl curl-devel openssl openssl-devel tzdata libxml2 libxml2-devel libxslt libxslt-devel sqlite-devel git postgresql-server postgresql postgresql-devel
```

### Create installation user and switch to it
```
root@deploy $ useradd user
root@deploy $ passwd user
root@deploy $ su - user
```

### Checkout the source code
First you clone the repository, then you check out the tag that you intend to deploy. This ensure that the version of the build scripts you are running is the same as the version of the application you intend to deploy. Upon checking out the tag you will see a warning about being in a detached HEAD state - this is expected as you have checked out a tag rather than a branch.
```
user@deploy $ git clone https://github.com/IntersectAustralia/dc21.git
user@deploy $ cd dc21
user@deploy $ git checkout <tag you wish to deploy>
```

### Install and use RVM
Again if you're having problems with this, visit: https://rvm.io/rvm/install/ for the latest.

```
user@deploy $ curl -L http://get.rvm.io | bash -s stable --ruby=1.9.2-p290
user@deploy $ source ~/.rvm/scripts/rvm
```

Create the gemset:

```
user@deploy $ rvm use 1.9.2-p290
user@deploy $ rvm gemset create dc21app
```

Change out and back into the dc21 directory. When you do this, the command line will prompt you to accept the rvmrc:
```
user@deploy $ cd ..
user@deploy $ cd dc21
```

Ensure that you are in the correct gemset by running the following command:
```
user@deploy $ rvm current
```

The output should be 'ruby-1.9.2-p290@dc21app'

Install bundler and the required environment for dc21
```
user@deploy $ gem install bundler
user@deploy $ bundle install
```

### Update production deployment configuration
Still in your _deploy_ machine, you need to set some configuration items to instruct the deployment scripts where the _server_ machine lives. From within the directory where you checked out the dc21 project (you should be there already), edit the file: ```config/deploy/production.rb```

* Supply your server's FQDN (hostname)
  * This will also be used to configure an Apache virtualhost
  * If you would not normally access the server via the name you supply here, you will need to manually edit /etc/httpd/conf.d/rails-dc21app.conf
  * In a standard deployment the web server, app server and database are all in the same place so these three entries will have the same values
* Set the el6 flag to false if using el5
* If you have a proxy server, fill in the details here.
  * If you have no proxy, leave it as 'nil'
  * If you do have a proxy, comment out the 'nil' line, and uncomment one of the other lines and update with your own settings.
  * The url should look like http://user:pass@proxy.example.com:8080
  * or without a username/pass: http://proxy.example.com:8080

```
# Your HTTP server, Apache/etc
set :web_server, 'hostname.com'
# This may be the same as your Web server
set :app_server, 'hostname.com'
# This is where Rails migrations will run
set :db_server, 'hostname.com'
# The user configured to run the rails app
set :user, 'dc21'
# If you are using RHEL/CentOS 6 or later, set this to true
set :el6, true
# If you have a proxy server, enter the address here including "inverted commas", eg:
#set :proxy, "http://user:pass@proxy.example.com:8080" # with a user/password
#set :proxy, "http://proxy.example.com:8080" # without a user/pass
set :proxy, nil
```

### If you have a proxy, apply proxy settings
```
user@deploy $ cap production server_setup:set_proxies
```

## Deploy The Application
Still on the _deploy_ server, the following steps are run to deploy the application.

### Deploy the app and create database tables
When deploying to production, you should deploy a "tag" (version) rather than HEAD in accordance with good release management practices.
* When running each of these tasks, you will be prompted for the sudo password for your dc21 unix user. This will be the first password prompt, as just: `Password: `
* When running deploy:update, you will also be prompted for the dc21 postgres user password. This will e the second password prompt, as : `Database password: `
* During deploy:update, you will be prompted for a tag to deploy. The possible tags are displayed. The development team should have informed you which tag to deploy.
* If deployment stalls running 'bundle:install' in deploy:update, this is likely due to a misconfigured proxy. The quick-fix solution is to copy the command in yellow and run it directly on the server as the dc21 user while the deploy is hung, then restart the deploy.

```
user@deploy $ cap production deploy:setup
user@deploy $ cap production deploy:update
user@deploy $ cap production deploy:schema_load
```

## Customise the application
You will now configure some local settings on your _server_ machine. After deploying the server for the first time, a file was created at `/data/dc21app_extra_config.yml` on the _server_ machine. This is where you configure settings specific to your installation.

Log in to your _server_ machine and edit the file `/data/dc21app_extra_config.yml`
```
dc21@server $ vi /data/dc21app_extra_config.yml
```

* hiev_handle_prefix (prefixes onto the ID for packages, defaults to hiev if not specified)
* notification_email_sender (The FROM: address in notification emails - e.g. password reset, account approval)
* column_mappings (The 'names' used in column mappings. See http://i.imgur.com/WUdbB.png for example)
* for_codes_lookup_base_url (URL of FOR code Server)
* for_codes_path (Local path for FOR code data)
* file_types (additional file types for selection when uploading - RAW is always included so don't add it here)
* ip_addresses (The white list of IP addresses that are allowed to access the download page of published packages without authentication).
* parameter_categories, parameter_units, parameter_modifications - the data for the lookup tables relating to experiment parameters (refer to user guide for more details)
* tags (the set of allowed tags for use on upload. You will need to restart the server for any updates made to the file to take effect.)

On subsequent deploys, this file will NOT be overwritten if it already exists in the server, so your settings will remain on future re-deploys.

## Seed the database and create an initial admin user
Run the following commands on your _deploy_ machine. This inserts data into the static lookup tables used by DC21, then creates an initial user account for logging in to the application.

* Similar to previous 'cap' commands, when running this task, you will first be prompted for the sudo password for your dc21 unix user. This prompt will appear first, as just: `Password: `
* You will then be prompted for some details about the initial user for the web application. Enter appropriate values for first name, last name and email. For the password, it must must be between 6 and 20 characters long and contain at least one uppercase letter, one lowercase letter, one digit and one symbol. Note: this is the user you will use when you first log in to the web application. It would be wise to change the password for this initial user when you first log in.

```
user@deploy $ cap production deploy:seed deploy:generate_initial_user
```


## Start the server
On your _server_ machine:
```
root@server $ service httpd start
```

## Configure the background workers
> Only applies for HIEv v1.9.01 or greater
Refer to [Setting up Redis and Resque](Setting_up_redis_and_resque.md)

## Configure the OAI-PMH Provider
Refer to [Setting Up OAI](Setting_Up_OAI.md)

## Optionally, set up SSL
Refer to [Setting Up SSL](Setting_Up_SSL.md)

## Check that things work
Check that you can successfully visit the site at the hostname or IP you configured.

If you have problems you can check both the Apache logs and also web application logs in the logs directory under the dc21app directory (e.g. under /home/dc21/dc21app/shared/log).

You can now proceed to run through the [Smoke Tests](Smoke_tests.md)
