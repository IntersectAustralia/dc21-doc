## File Upload
Files can be uploaded via the API one file at a time. Construct a multi-part post request with the required parameters and file attachment, as per the details below. 

Authentication is done by adding your API token to the URL. API tokens can only be used for API actions, however you should still take care to protect your token, as it could be used to fraudulently upload files on your behalf if obtained by someone else. You can obtain an API token by clicking on your email address, the "Settings" at the top right of the DC21 application.

### Request

* Verb: POST
* URL: https://\<base_url_for_your_server\>/data_files/api_create.json?auth_token=\<your_personal_api_token\>
* Mandatory POST parameters:
  * **experiment_id** (or **org_level2_id**) - the numeric ID of the experiment this file belongs to - you can find the numeric ID on the 'view experiment page'
  * **type** - the type of file, one of UNKNOWN, RAW, CLEANSED, PROCESSED
defined in DC21
  * **file** - the actual file
* Optional POST parameters:
  * **description** - a description of the file
  * **tag_names** - a quoted, comma separated list of tags to apply to the file, must be from the set of legal tag names 
  * **parent_file_names** - a quoted, comma separated list of parent file names of the file
  * **label_names** - a quoted, comma separated list of labels to apply to the file


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
  have permissions to upload</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>Invalid input - any combination of the
  following:<br>
  - file not supplied<br>
  - type not supplied<br>
  - experiment id not supplied<br>
  - unrecognised type<br>
  - experiment does not exist<br>
  - file element is not a valid file (e.g. a string was
  supplied instead of a file part)</td>
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
  <td>Success - the file was accepted and no problems were detected</td>
  <td>200</td>
  <td>json hash as per below, messages will be empty</td>
 </tr>
 <tr>
  <td>Warning scenarios (the file has been
  accepted but there's warnings to be considered) - various possible
  combinations of<br>
  - file already existed and was renamed<br>
  - safe overlap replacement<br>
  - bad overlap (file type goes to error)<br>
  </td>
  <td>200</td>
  <td>json hash e.g.<br>
<div class="highlight">
<pre>
{
 "file_id":the_file_id, 
 "messages":[array_of_messages], 
 "file_name":"resulting_file_name", 
 "file_type":"resulting_type"
}
</pre>
</div>
</td>
 </tr>
</table>


### Example
We highly recommend the [rest client](https://github.com/archiloque/rest-client) gem if calling the API from Ruby. The below example uses rest client to send the request.

```ruby
file = File.new('/Users/georgina/projects/dc21/samples/full_files/weather_station/weather_station_05_min.dat')
params = {:file => file, :experiment_id => 78, :type => 'RAW'}
url = 'http://localhost:3000/data_files/api_create?auth_token=RfmknM43yYnZxtVPfAuH'
response = RestClient.post url, params
```

Similar libraries exist in other languages (e.g. HttpClient for Java), plus there are various command line tools such as curl which could also be used.
