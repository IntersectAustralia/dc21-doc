## Assumptions
The
[Deployment Guide : First Time Server Build](Deployment_Guide_-_First_Time_Server_Build.md) instructions have been successfully followed. This means you will have a _deploy_ machine and a _server_ machine successfully built.

You should know which is your _deploy_ machine, and which is your _server_ machine, and will need the password for the "dc21" unix user that was created during the first time server build.

## Obtain a certificate
You will need to obtain a valid SSL certificate for your server from a commercial certificate provider.

For testing purposes, you can generate a self signed certificate, however this will generate warnings in the browser. To generate a self-signed certificate follow steps 1-4 of these instructions: http://www.akadia.com/services/ssh_test_certificate.html

## Install SSL certificate
SSH into your _server_ machine as root, and move the certificate files to the appropriate destinations. The certificate file (e.g. `server.crt`) should be moved to the `/etc/pki/tls/certs/` directory, and the certificate key (e.g. `server.key`) should be moved to `/etc/pki/tls/private/`.


## Configure Apache
Still on your _server_ machine, edit the Apache configuration file for the rails application (e.g. `/etc/httpd/conf.d/rails_dc21app.conf`)
The following is a template for a typical configuration. Please substitute:

* `#server url#` for your server's url.
* `#rails environment#` for the rails environment, for example, `production` or `staging`.
* `#certificate file#` for the path to the certificate file you installed previously.
* `#certificate key file#` for the path to the certificate key file you installed previously.
* `#passenger_version#` for your passenger's directory.
`#dc21_user#` for your installation user home directory.

Please note the trailing slash in the `Redirect permanent` entry.
```
LoadModule passenger_module /home/#dc21_user#/.rvm//gems/ruby-1.9.2-p290@dc21app/gems/#passenger_version#/ext/apache2/mod_passenger.so
PassengerRoot /home/#dc21_user#/.rvm//gems/ruby-1.9.2-p290@dc21app/gems/#passenger_version#
PassengerRuby /home/#dc21_user#/.rvm//wrappers/ruby-1.9.2-p290@dc21app/ruby
PassengerTempDir /home/#dc21_user#/dc21app/current/tmp/pids

<VirtualHost *:80>
    ServerName #server url#
    Redirect permanent / https://#server url#/
</VirtualHost>

<VirtualHost _default_:443>
    ServerName #server url#
    RailsEnv #rails environment#

    DocumentRoot /home/#dc21_user#/dc21app/current/public

    SSLEngine on

    ErrorLog logs/ssl_error_log
    CustomLog logs/ssl_access_log combined
    CustomLog logs/ssl_request_log \
          "%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b"
    LogLevel warn

    SetEnvIf User-Agent ".*MSIE.*" \
         nokeepalive ssl-unclean-shutdown \
         downgrade-1.0 force-response-1.0

    SSLProtocol all -SSLv2
    SSLCipherSuite HIGH:MEDIUM
    SSLCertificateFile #certificate file#
    SSLCertificateKeyFile #certificate key file#

    XSendFile on
    XSendFilePath /tmp
    XSendFilePath /data/dc21-data

    <Directory /home/#dc21_user#/dc21app/current/public>
        AllowOverride all
        Options -MultiViews
    </Directory>
</VirtualHost>
```

For example, the configuration for our staging server looks like:
```
LoadModule passenger_module /home/devel/.rvm//gems/ruby-1.9.2-p290@dc21app/gems/passenger-3.0.11/ext/apache2/mod_passenger.so
PassengerRoot /home/devel/.rvm//gems/ruby-1.9.2-p290@dc21app/gems/passenger-3.0.11
PassengerRuby /home/devel/.rvm//wrappers/ruby-1.9.2-p290@dc21app/ruby
PassengerTempDir /home/devel/dc21app/current/tmp/pids

<VirtualHost *:80>
    ServerName jp-dc21-staging.intersect.org.au
    Redirect permanent / https://jp-dc21-staging.intersect.org.au/
</VirtualHost>

<VirtualHost _default_:443>
    ServerName jp-dc21-staging.intersect.org.au
    RailsEnv staging

    DocumentRoot /home/devel/dc21app/current/public

    SSLEngine on

    ErrorLog logs/ssl_error_log
    CustomLog logs/ssl_access_log combined
    CustomLog logs/ssl_request_log \
          "%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b"
    LogLevel warn

    SetEnvIf User-Agent ".*MSIE.*" \
         nokeepalive ssl-unclean-shutdown \
         downgrade-1.0 force-response-1.0

    SSLProtocol all -SSLv2
    SSLCipherSuite HIGH:MEDIUM
    SSLCertificateFile /etc/pki/tls/certs/server.crt
    SSLCertificateKeyFile /etc/pki/tls/private/server.key

    XSendFile on
    XSendFilePath /tmp
    XSendFilePath /data/dc21-data

    <Directory /home/devel/dc21app/current/public>
        AllowOverride all
        Options -MultiViews
    </Directory>
</VirtualHost>
```

## Restart Apache & Test
Still on your _server_ machine, restart the apache service after performing these changes.
```
root@server $ service httpd restart
```
You can then test that you can visit the site at https, and that visiting http redirects to https.

## Next Steps
You can now proceed to run through the [Smoke Tests](Smoke_tests.md).
