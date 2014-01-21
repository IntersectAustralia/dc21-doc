# Registering your server with AAF

### Note: This does not use Rapid Connect. Rapid Connect support will be included in a future update.

## Assumptions
The [Deployment Guide : First Time Server Build](Deployment_Guide_-_First_Time_Server_Build.md) or [Upgrading From 1.9.04 to 2.0.01](Upgrading_From_1.9.04_to_2.0.01.md) instructions have been successfully followed. This means you will have a _server_ machine successfully built.

You should have been presented with the AAF certificate to register your app with. Copy this for use in Step 3.

The attributes to be requested are:
* auEduPersonTargetedID
* displayName
* email (required)
* givenName (required)
* surname (required)

1. Visit the [Production Registry](https://manager.aaf.edu.au/federationregistry/membership/serviceprovider/create) or the [Test Registry](https://manager.test.aaf.edu.au/federationregistry/membership/serviceprovider/create)
2. In the "SAML Configuration" section, choose the "Shibboleth Service Provider (2.4.x)" and enter the URL of the server where the SP will be running (e.g https://dc21-instance.example.com).
3. In the "Public Key Certificate" section, paste the certificate displayed during the server build/upgrade process. It should print out the Subject and Issuer as the hostname you set up.
4. In the "Requested Attributes" section, check the "Requested" boxes for the attributes listed above and provide a reason (eg. Identify user in DC21 instance). Make sure to check the "Required" boxes for the marked attributes listed above.
5. Click "Submit" and await approval.
