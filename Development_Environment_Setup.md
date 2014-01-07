If you haven't already, you will need to set up your computer for Rails development. We highly recommend Mac or Linux over Windows as a development environment. If you are on Windows, we recommend running Linux in a virtual machine for development. The below instructions cover CentOS 6 or Mac OSX, but can easily be adapted for other Linux distributions.

## Assumptions
This assumes familiarity with a *nix environment, particularly running commands in a terminal, as well as basic knowledge of Git and PostgreSQL. We assume you have a user account set up with the ability to sudo to root. In the below snippets where we show `username@devmachine` this means you are logged in with your own username on your dev machine and have a terminal open to run the command.

## Rails Environment Setup

### Packages (CentOS only)
Install some packages you'll be needing
```
username@devmachine $ sudo yum install -y gcc ruby rubgems ruby-devel libxml2 libxml2-devel libxslt libxslt-devel git
```
### RVM
Install RVM (see https://rvm.io/rvm/install/ for the latest - steps below may change over time)
```
username@devmachine $ curl -L https://get.rvm.io | bash -s stable
```
Reopen your terminal and check that RVM is working correctly
```
username@devmachine $ type rvm | head -n 1
# should print 'rvm is a function'
```
If this doesn't work and you're using Gnome terminal, follow these instructions: https://rvm.io/integration/gnome-terminal/ . Otherwise visit https://rvm.io/rvm/install/ for troubleshooting.

Use RVM to find out its requirements for your OS, and follow the instructions it provides. This will include installing some packages.
```
username@devmachine $ rvm requirements
# install the packages it lists for Ruby
```
Note (CentOS only): If an error message pops up saying "Error installing EPEL, it is required for libyaml-devel", try using the following commands
```
username@devmachine $ curl -O http://www6.atomicorp.com/channels/atomic/centos/6/x86_64/RPMS/atomic-release-1.0-14.el6.art.noarch.rpm
username@devmachine $ sudo  rpm -Uvh atomic-release-1.0-14.el6.art.noarch.rpm
username@devmachine $ sudo yum install libyaml-devel
```
Install Ruby 1.9.2
```
username@devmachine $ rvm install 1.9.2-p290
```
### Postgres (CentOS)
```
username@devmachine $ sudo yum install -y postgresql-server postgresql postgresql-devel
username@devmachine $ sudo service postgresql initdb
```
Change to use password authentication - as the postgres user, edit `/var/lib/pgsql/data/pg_hba.conf`,
```
username@devmachine $ sudo su - postgres
postgres@devmachine $ vi /var/lib/pgsql/data/pg_hba.conf
```
change the last few lines to look like this:
```
# "local" is for Unix domain socket connections only
local   all         all                               trust
# IPv4 local connections:
host    all         all         127.0.0.1/32          password
# IPv6 local connections:
host    all         all         ::1/128               password
```
Exit out of the postgres account, then start Postgres and enable services at boot:
```
postgres@devmachine $ exit
username@devmachine $ sudo service postgresql start
username@devmachine $ sudo /sbin/chkconfig postgresql on
```
### Postgres (Mac)

We recommend using [Homebrew](https://github.com/Homebrew/homebrew) 
```
brew install postgresql
# then follow the instructions it provides
```

Alternatively, remove the version that comes with Mac OS X use [Postgres.app](http://postgresapp.com/).
```
which psql
rm /usr/bin/psql
```

## DC21 Project
### Set up DC21 postgres user
```
username@devmachine $ sudo su - postgres
postgres@devmachine $ createuser -P dc21
$ => Enter password for new role: dc21
$ => Enter it again: dc21
Shall the new role be a superuser? (y/n) n
Shall the new role be allowed to create databases? (y/n) y
Shall the new role be allowed to create more new roles? (y/n) n
postgres@devmachine $ exit
```
### Fork and clone the repository
Unless you have push access to the Intersect repository, you will want to create your own fork of the repository. See https://help.github.com/articles/fork-a-repo for help with forking a repository. Once you have a fork, clone that into a suitable directory on your local machine.
```
# Clone your fork of the repo into the current directory in terminal
username@devmachine $ git clone https://github.com/<yourusername>/dc21.git
username@devmachine $ cd dc21  
# you will prompted to accept the .rvmrc only this first time. Review the file, then accept the prompt.
```
### Install required gems
```
username@devmachine $ sudo gem install bundler
username@devmachine $ bundle install
```
### Create a directory for uploaded files to go into
```
username@devmachine $ sudo mkdir -p /data/dc21-data
username@devmachine $ sudo chmod go+w /data/dc21-data
```
### Create database, run seed and populate scripts
This creates the database tables, inserts seed data and populates it with some test data in your local dev environment.
```
username@devmachine $ bundle exec rake db:setup db:seed db:populate
# you will be prompted for a sample user password, this will be the password 
# for all the sample user accounts that get created by default. Pick something
# you will remember, it must be between 6 and 20 characters long and contain
# at least one uppercase letter, one lowercase letter, one digit and one symbol.
```
### Start up a local server
Start you local server, then have a look around at http://localhost:3000 - you can log in as admin@intersect.org.au with the password you set above
```
username@devmachine $ rails s
```
