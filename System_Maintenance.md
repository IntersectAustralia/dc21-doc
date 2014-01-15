# System Maintenance

### OS patching

Please follow your institution's best practices regarding the patching of your operating system.

### System Backups
It is recommended that the server where the application runs is backed up regularly.
The following directories should be backed up:
* Files roots, which are defined in the config/dc21app_config.yml file
* application user home page, e.g. /home/dc21
* Apache configuration files, e.g. /etc/httpd/
* Database files, e.g. /var/lib/pgsql/data
* Other system files as defined in your institution best practices

### Rails Security Advisories
The best way to keep informed about Rails Secury Advisories is to subscribe to the Ruby on Rails security group: http://groups.google.com/group/rubyonrails-security

For each security vulnerability there will be a minor version release that fixes the problem. Is is up to the system administrator to evaluate the security vulnerability, and how critical it is to perform an upgrade.

In case an upgrade is deemed necessary, please update the version of rails in the Gemfile and run a bundle install. Then run all cucumber and rspec tests, and make sure they pass. Run the server locally, and make sure the user interface and basic functionality works as expected

### Errors
You can monitor the application logs for errors. This can be achieved through manual crontab scripts that analyse the logs, or through SAAS tools like https://airbrake.io/pages/home

The apache log files are rotated as configured in the config/dc21app.logrotate file. You can customize that file to perform the log rotation you consider appropriate.

### Availability
It is recommended to set up a tool to monitor the website availability. This could be an in-house built tool, running test scripts through cronjobs, or similar.

Alternatively, it is possible to use a SAAS tool like Nagios (http://www.nagios.org/), StillAlive (https://stillalive.com/) or similar.

### Disk Space and Performance monitoring
It is recommended to set up a tool to monitor the system performance, both from the point of view of the web application and the point of view of the system itself. Things to check would be CPU usage, Disk utilization, page load times, etc...

Again this could be achieved with some set of manual scripts running through crontab, though in this case, the list of metrics to measure can be relatively large, so this would be significant amount of work.

Another alternative, again, is using a SAAS tool, like http://newrelic.com/ that, for a fee, will offer good insight into the performance of your system.

The principal paths to monitor for disk usage are the volumes where the following paths are mounted:

* The files roots, which are defined in the config/dc21app_config.yml file.
* Apache log files directory.
* Postgress database directories
* Application user home directory
