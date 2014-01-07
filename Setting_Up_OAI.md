## Assumptions
The 
[Deployment Guide : First Time Server Build](https://github.com/IntersectAustralia/dc21/wiki/Deployment-Guide-:-First-Time-Server-Build) instructions have been successfully followed. This means you will have a _deploy_ machine and a _server_ machine successfully built.

You should know which is your _deploy_ machine, and which is your _server_ machine, and will need the password for the "dc21" unix user that was created during the first time server build.

## Java install
SSH into your _server_ machine as root, then install java:
```
root@server $ yum install java-1.6.0-openjdk java-1.6.0-openjdk-devel
```
## JOAI install
SSH into your _deployment_ server as the deployment user and install joai by running the following commands.
* When running the 'cap' task, you will first be prompted for the sudo password for your dc21 unix user. This prompt will appear first, as just: `Password: `
* You will also be prompted to choose a new jOAI password. This prompt appears second, as : `New jOAI password (at least six alphanumeric characters):`

```
user@deploy $ cd dc21
user@deploy $ cap production joai:deploy
```

The installation process sets up tomcat as a service, and configures apache as a reverse proxy for tomcat. You may wish to check the configuration and customise it according to your best practices. The service configuration can be found at `/etc/init.d/tomcat` and the apache configuration can be found at `/etc/httpd/conf.d/tomcat_joai.conf`

## Start tomcat
On your _server_ machine, start the tomcat service and re-start apache:
```
root@server $ service tomcat start
root@server $ service httpd restart
```

## Configure RIF-CS Harvest
1. Visit "http://server_url/oai"
2. Go to "Data Provider -> Repository Information and Administration". Log in with the user "oai_admin" and the password you set in previous step. Update the repository information:
![jOAI Setup 1](https://github.com/IntersectAustralia/dc21/raw/master/doc/joai%20setup%201.png)
3. You can find the dc21 metadata configuration under "Data Provider -> Metadata Files Configuration". Please confirm that it looks correct and reindex it, by clicking the "Reindex" button under the Action column.

Any other optional settings you may be interested in is documented in detail here: http://www.ands.org.au/resource/joai/joai.html

## Next Steps
Optionally, set up SSL - refer to [[Setting Up SSL]], otherwise you can now proceed to run through the [[Smoke Tests]].