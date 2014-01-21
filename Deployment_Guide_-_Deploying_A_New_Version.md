# Deployment Guide: Deploying a new version

## Assumptions
The [Deployment Guide : First Time Server Build](Deployment_Guide_-_First_Time_Server_Build.md) or [Upgrading From 1.9.04 to 2.0.01](Upgrading_From_1.9.04_to_2.0.01.md) instructions have been successfully followed. This means you will have a _server_ machine successfully built.


## Updating Your System
Go to your deployment directory (i.e. the directory you cloned the repository to during original setup), e.g. assuming you checked it out to your home directory:
```
dc21@server $ source $HOME/setup_config
dc21@server $ cd ~/code_base/dc21
```
Update your copy of the repo and check out the tag you want to deploy:
```
dc21@server $ git checkout master
dc21@server $ git pull
dc21@server $ git checkout <tag you want to deploy>
```
Rebundle on your local machine
```
dc21@server $ bundle install
```
Redeploy the code
* You will be prompted for a tag to deploy. The possible tags are displayed. The development team should have informed you which tag to deploy.
* You will be prompted for the sudo password for your dc21 unix user. This will be the first password prompt, as just: `Password: `

```
dc21@server $ cap local deploy:safe
```

### Smoke Tests
You may now wish to run the [Smoke Tests](Smoke_tests.md) to ensure that all parts of the system are running and communicating as expected.
