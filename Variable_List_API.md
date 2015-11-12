# Variable List API

## Variable List
DIVER variables from uploaded TOA5, NETCDF and NCML files plus column mappings can be extracted using this API. To use this API, construct a post request as per the details below.

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

### Structure of the returned JSON Body
If the API call is successful, a JSON object will be returned. This will consist of a list of dictionaries with each dictionary representing a single variable. Each variable will have the following entries:
- created_at: the date/time when the variable's description was created in DIVER
- date_file_id : the host file ID
- data_type : the type of data eg: Avg
- fill_value : the value that should be displayed for the variable if it is null
- id : the variable's unique identifier within DIVER
- name : the variable's name, eg: latitude
- position : an integer showing the column # of the variable within the host file
- unit : the unit of the variable, eg: m, degrees_North
- updated_at : the date/time when the variable's description was last updated in DIVER

### Sample JSON Output
[{"created_at":"2015-11-09T12:17:40+11:00","data_file_id":53,"data_type":null,"fill_value":null,"id":170,"name":"latitude","position":1,"unit":"degrees_north","updated_at":"2015-11-09T12:17:40+11:00"},
{"created_at":"2015-11-09T11:57:31+11:00","data_file_id":47,"data_type":null,"fill_value":null,"id":138,"name":"levels","position":2,"unit":"m","updated_at":"2015-11-09T11:57:31+11:00"},
{"created_at":"2015-11-09T12:26:16+11:00","data_file_id":57,"data_type":"Avg","fill_value":null,"id":215,"name":"VW_Avg(1)","position":2,"unit":"","updated_at":"2015-11-09T12:26:16+11:00"},
{"created_at":"2015-11-09T14:44:08+11:00","data_file_id":67,"data_type":"Avg","fill_value":null,"id":902,"name":"VW_Avg(1)","position":2,"unit":"","updated_at":"2015-11-09T14:44:08+11:00"}]
