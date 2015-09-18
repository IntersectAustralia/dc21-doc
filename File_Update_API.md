# File Update API

Metadata of files can be updated via the API. Construct a post request with the required parameters, as per the details below.

Authentication is done by adding your API token to the URL. API tokens can only be used for API actions, however you should still take care to protect your token, as it could be used to fraudulently update files on your behalf if obtained by someone else. You can obtain an API token by clicking on your email address, the "Settings" at the top right of the DC21 application.

### Request

* Verb: POST
* URL: https://\<base_url_for_your_server\>/data_files/api_update?auth_token=\<your_personal_api_token\>
* Mandatory POST parameters:
  * **file_id** - the id of the file to be updated
* Optional POST parameters:
  * **name** - the name of the file
  * **experiment_id** (or **org_level2_id**) - the numeric ID of the experiment this file belongs to - you can find the numeric ID on the 'view experiment page'
  * **description** - a description of the file
  * **tag_names** - a quoted, comma separated list of tags to apply to the file, must be from the set of legal tag names
  * **parent_filenames** - an array of parent file names, which must exist on the server prior to update
  * **label_names** - a quoted, comma separated list of labels to apply to the file
  * **title** - the title of the package
  * **grant_numbers** - a quoted, comma separated list of grant numbers to apply to the package
  * **related_websites** - a quoted, comma separated list of related websites to apply to the package
  * **access_rights_type** - the access rights type, must be one of "Open", "Conditional" or "Restricted"
  * **license** - the license, must be one of "CC-BY", "CC-BY-SA", "CC-BY-ND", "CC-BY-NC", "CC-BY-NC-SA", "CC-BY-NC-ND" or "All rights reserved"
  * **access** - access level for the file is either 'Public' or 'Private' (default is 'Private' unless otherwise specified)
  * **access_to_all_institutional_users** - an option for private access for Institutional Users, which can be set either true or false
  * **access_to_user_groups** - an option for private access for certain groups of users, which can be set to true or false
  * **access_groups** - an array of names of access groups, which must exist on the server prior to update
  * **start_time** - The start time to use if one could not be extracted from the file's metadata. Must be in the format 'yyyy-mm-dd hh:mm:ss'
  * **end_time** - The end time to use if one could not be extracted from the file's metadata. Must be in the format 'yyyy-mm-dd hh:mm:ss'

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
  have permissions to update</td>
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
  <td>Invalid input - any combination of the
  following:<br>
  - file_id not supplied<br>
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
  <td>Success - the file was updated and no problems were detected</td>
  <td>200</td>
  <td>json hash as per below</td>
 </tr>
 <tr>
  <td>Warning scenarios (the file has been
  accepted but there's warnings to be considered) - various possible
  combinations of<br>
  - updates to a package are noto going to be reflected in the rif-cs
  </td>
  <td>200</td>
  <td>json hash e.g.<br>
<div class="highlight">
<pre>
{
 "messages":[array_of_messages],
 "warnings":[array_of_warnings]
}
</pre>
</div>
</td>
 </tr>
</table>


### Example
We highly recommend the [rest client](https://github.com/archiloque/rest-client) gem if calling the API from Ruby. The below example uses rest client to send the request.

```ruby
params = {:file_id => 10, :experiment_id => 78}
url = 'http://localhost:3000/data_files/api_update?auth_token=RfmknM43yYnZxtVPfAuH'
response = RestClient.post url, params
```

Similar libraries exist in other languages (e.g. HttpClient for Java), plus there are various command line tools such as curl which could also be used.
