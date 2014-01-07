## Assumptions
The 
[Deployment Guide : First Time Server Build](https://github.com/IntersectAustralia/dc21/wiki/Deployment-Guide-:-First-Time-Server-Build) instructions have been successfully followed. This means you will have a _deploy_ machine and a _server_ machine successfully built.

You should know which is your _deploy_ machine, and which is your _server_ machine. You will also need the password for the "dc21" unix user and "dc21" postgres user that was created during the first time server build.

If you don't have a deployment machine, or need a new one, follow the instructions under "Setting up the deployment machine" on the initial setup page: [[Deployment Guide : First Time Server Build]]

## Updating Your System
Go to your deployment directory (i.e. the directory you cloned the repository to during original setup), e.g. assuming you checked it out to your home directory:
```
user@deploy $ cd ~/dc21
```
Update your copy of the repo and check out the tag you want to deploy:
```
user@deploy $ git checkout master
user@deploy $ git pull
user@deploy $ git checkout <tag you want to deploy>
```
Rebundle on your local machine
```
user@deploy $ bundle install
```
Redeploy the code
* You will be prompted for a tag to deploy. The possible tags are displayed. The development team should have informed you which tag to deploy.
* You will be prompted for the sudo password for your dc21 unix user. This will be the first password prompt, as just: `Password: `
* You will also be prompted for the dc21 postgres user password. This will be the second password prompt, as : `Database password: `

```
user@deploy $ cap production deploy:safe
```

### Upgrading to 1.9.01 or greater
You need to run the following instructions when upgrading for 1.8.nn to 1.9.nn to setup background workers implemented in 1.9.01.

#### Setting up Redis/Resque
Refer to [[Setting-up-redis-and-resque]]

#### Restarting Resque
Resque needs to be restarted every time a new version is deployed. To do this, login to the server and then in the project root directory:
```
$ bundle exec rake daemon:resque:stop
$ bundle exec rake daemon:resque:start RAILS_ENV=<env>
```
Where is your target environment, eg. qa, staging, production.

### Smoke Tests
You may now wish to run the [[Smoke Tests]] to ensure that all parts of the system are running and communicating as expected.