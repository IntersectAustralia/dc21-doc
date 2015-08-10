# Package Publish API

Packages can be published via the API. Construct a post request with the required parameters, as per the details below.

The Package Publish API is only available to administrators.
Authentication is done by adding your API token to the URL. API tokens can only be used for API actions, however you should still take care to protect your token, as it could be used to fraudulently upload files on your behalf if obtained by someone else. You can obtain an API token by clicking on your email address, the "Settings" at the top right of the DC21 application.

### Request

* Verb: POST
* URL: https://\<base_url_for_your_server\>/packages/api_publish?auth_token=\<your_personal_api_token\>
* Mandatory POST parameters:
  * **package_id** - the file ID of the package that should be published. You can find packages you want to publish by using the [Search API](Search_API.md)

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
   <td>Unauthorized</td>
   <td>401</td>
   <td>empty</td>
  </tr>
 <tr>
  <td>Invalid input - any combination of the following:<br>
  - package id is required<br>
  - package with id could not be found<br>
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
  <td>Success - the package was published</td>
  <td>200</td>
  <td>json hash, messages will contain information about how the package was created
<div class="highlight">
<pre>
{
 "package_id":the_package_id,
 "messages":[array_of_messages]
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

url = "http://<host:port>/packages/api_publish" # Please change the host:port part!
params = {"auth_token"=>"RQdasxNyCdzzZoNGfPvw", "package_id"=>"3" } # Generate your own token and paste here
response = RestClient.post(url, params)

puts response
```

Similar libraries exist in other languages (e.g. HttpClient for Java), plus there are various command line tools such as curl which could also be used.
