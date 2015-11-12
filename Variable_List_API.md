# Variable List API

## Variable List
Current DIVER variables and column mappings can be extracted using this API. Construct a post request as per the details below.

Authentication is done by including your API token into post parameters. API tokens can only be used for API actions. You can obtain an API token by clicking on your email address, the "Settings" at the top right of the DC21 application.

### Request

* Verb: POST
* URL: https://\<base_url_for_your_server\>/data_files/variable_list
* Mandatory POST parameters:
  * **auth_token** - authentication token
* Optional POST parameters:
  * (Nil)

### Response
The result is reported back via a combination of the HTTP response code, and a JSON body.
<table>
 <tr>
  <th>Scenario</th>
  <th>Response Code</th>
  <th>Body</th>
 </tr>
 <tr>
  <td>No authentication token supplied</td>
  <td>401</td>
  <td>empty</td>
 </tr>
 <tr>
  <td>Unrecognised authentication token supplied</td>
  <td>401</td>
  <td>empty</td>
 </tr>
 <tr>
  <td>Valid authentication token supplied but user does not
  have permissions to use this API</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>Success</td>
  <td>200</td>
  <td>json hash as per below</td>
 </tr>
</table>


### Example
We highly recommend the [rest-client](https://github.com/rest-client/rest-client) gem if calling the API from Ruby. The below example uses rest client to send the request.

```ruby
#!/usr/bin/env ruby

require 'rest_client'

url = "http://<host:port>/data_files/variable_list" # Please change the host:port part!
params = {"auth_token"=>"RQdasxNyCdzzZoNGfPvw"} # Generate your own token and paste here
response = RestClient.post(url, params)

puts response
```

Similar libraries exist in other languages (e.g. HttpClient for Java), plus there are various command line tools such as curl which could also be used.

Here is a example of unix curl command:

```bash
$ curl -X POST -d "stati=RAW&auth_token=RQdasxNyCdzzZoNGfPvw" http://<host:port>/data_files/variable_list
```

The command line options used here:
```
-X, --request <command>
-d, --data <data>
-k, --insecure
```

Other options and arguments please refer to manpage of curl(1)

## JSON

If the API call is successful, a JSON object will be returned ..., where each data file that matched the criteria will contain a `url` value, eg. http://\<host\:port\>/data_files/21/download in 1.8.02 and http://\<host\:port\>/data_files/21/download.json in 1.9.01 onwards

### Request

