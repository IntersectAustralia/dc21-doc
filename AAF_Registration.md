# Registering your server with AAF

## Assumptions
You have defined a hostname to use for the final application. eg. diver.intersect.org.au

| Field | Comment | Example |
----|----|----
| Organisation| Owner of DIVER instance | University of Western Sydney |
|Name| Name of Service Provider | Example DIVER instance |
|URL| Full hostname with https:// protocol. SSL is required for AAF authentication. | https://www.example.com/ |
|Callback URL | This will always be in the form of https://hostname/users/aaf_sign_in | https://www.example.com/users/aaf_sign_in |
|Secret | Remember this token. It will be used in the deploy config. | See [AAF Rapid Connect](https://rapid.aaf.edu.au/registration) |

The process can take a few days before your Service Provider is approved by the AAF Production Federation. When it is approved, you will be emailed a login URL in the form `https://rapid.aaf.edu.au/jwt/authnrequest/research/REPLACE_ME` which will be used later in the deploy script.

In the setup_config file, you will need the following variables if they are not defined already:

```
# The token you generated when registering for AAF Rapid Connect. You will need to escape double quotes in the token.
export AAF_SECRET_TOKEN=""
# The URL will be emailed to you when your service provider is approved in the Production Federation
export AAF_LOGIN_URL="https://rapid.aaf.edu.au/jwt/authnrequest/research/REPLACE_ME"

```
