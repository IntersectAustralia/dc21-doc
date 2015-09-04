# Package Create API

Packages can be created via the API. Construct a post request with the required parameters, as per the details below.

Authentication is done by adding your API token to the URL. API tokens can only be used for API actions, however you should still take care to protect your token, as it could be used to fraudulently upload files on your behalf if obtained by someone else. You can obtain an API token by clicking on your email address, the "Settings" at the top right of the DC21 application.

### Request

* Verb: POST
* URL: https://\<base_url_for_your_server\>/packages/api_create?auth_token=\<your_personal_api_token\>
* Mandatory POST parameters:
  * **file_ids** - an array of numeric file ids that should be included in the package - you can find files you want to package by using the [Search API](Search_API.md)
  * **filename** - the filename of the package to create (excluding .zip)
  * **experiment_id** (or **org_level2_id**) - the numeric ID of the experiment this package belongs to - you can find the numeric ID on the 'view experiment page'
  * **title** - the title of the package
  * **access_rights_type** - the access rights type, must be one of "Open", "Conditional" or "Restricted"
* Optional POST parameters:
  * **description** - a description of the package
  * **license** - the license, must be one of "CC-BY", "CC-BY-SA", "CC-BY-ND", "CC-BY-NC", "CC-BY-NC-SA", "CC-BY-NC-ND" or "All rights reserved"
  * **tag_names** - a quoted, comma separated list of tags to apply to the package, must be from the set of legal tag names
  * **label_names** - a quoted, comma separated list of labels to apply to the package
  * **grant_numbers** - a quoted, comma separated list of grant numbers to apply to the package
  * **related_websites** - a quoted, comma separated list of related websites to apply to the package
  * **start_time** - the start time to use. Must be in the format 'yyyy-mm-dd hh:mm:ss'
  * **end_time** - the end time to use. Must be in the format 'yyyy-mm-dd hh:mm:ss'
  * **run_in_background** - use "false" if the package should be built in the foreground (defaults to "true")
  * **force** - use "true" to allow including error files or incomplete packages in package creating. (defaults to "false")

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
  <td>Invalid input - any combination of the following:<br>
  - file with id does not exist<br>
  - file id is invalid<br>
  - file ids not supplied<br>
  - filename not supplied<br>
  - filename already exists<br>
  - title not supplied<br>
  - experiment id not supplied<br>
  - experiment does not exist<br>
  - tag cannot be found<br>
  - start or end date is not valid<br>
  - start date is after end date<br>
  </td>
  <td>400</td>
  <td>json hash e.g.<br>
<div class="hightlight">
<pre>
  {
    "messages":[array_of_messages]
  }
</pre>
</div>
  </td>
 </tr>
 <tr>
  <td>Success - the package was created</td>
  <td>200</td>
  <td>json hash, messages will contain information about how the package was created
<div class="highlight">
<pre>
{
 "package_id":the_package_id,
 "messages":[array_of_messages],
 "file_name":"resulting_package_name.zip",
 "file_type":"PACKAGE"
}
</pre>
</div>
</td>
 </tr>
</table>

### Example
We highly recommend the [rest-client](https://github.com/rest-client/rest-client) gem if calling the API from Ruby. The below example uses rest client to send the request.

```ruby
#!/usr/bin/env ruby

require 'rest_client'

url = "http://<host:port>/packages/api_create" # Please change the host:port part!
params = {"auth_token"=>"RQdasxNyCdzzZoNGfPvw", "file_ids"=>["1","2","3"], "filename"=>"my_package", "experiment_id"=>"1", "title"=>"My First Package" } # Generate your own token and paste here
response = RestClient.post(url, params)

puts response
```

Similar libraries exist in other languages (e.g. HttpClient for Java), plus there are various command line tools such as curl which could also be used.
