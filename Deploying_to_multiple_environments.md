This information assumes you have a basic working knowledge of linux systems admin as well as Capistrano. If you are at the point of wanting multiple environments we recommend spending some time to understand Capistrano and the deployment process so you can make your own decisions about the best model for your team and environment. You may wish to fork the repository to make changes to suit your circumstances.

Note that 'development' and 'test' are special environments used for local dev and running automated tests respectively. You shouldn't modify these.

# Background
If you want to run multiple environments (e.g. to have a test and a production server), DC21 support this through the use of the [Capistrano Multistage Extension](https://github.com/capistrano/capistrano/wiki/2.x-Multistage-Extension). Please have a read to familiarise.

The set of possible environments is configured in `config/deploy.rb` - for example:
```
set :stages, %w(qa staging production)
```

Each environment then needs a number of additional things configured - see below.

# Setup
If you want a new named environment, there's a number of places you need to add the new environment name
* edit `config/deploy.rb` - edit the set of stages if you want something in addition to qa, staging, production
* edit `config/database.yml` - add your new environment
* edit `config/dc21app_config.yml` - add your new environment, you can just inherit default as per others
* edit `deploy_templates/dc21app_extra_config.yml` - add your new environment, you can just inherit default as per others
* create `config/deploy/<yourenv>.rb` - create a new file named for your environment, copying and adjusting settings as needed from another environment. More info about how to configure this can be found in the section titled "Update production deployment configuration" in [Deployment Guide : First Time Server Build](Deployment_Guide_-_First_Time_Server_Build.md)
* create `config/environments/<yourenv>.rb` - create a new file named for your environment, copying and adjusting settings as needed from another environment

# Running
Each time you run a cap task, specify the environment name, e.g.
```
cap production deploy:setup
OR
cap staging deploy:setup
```
