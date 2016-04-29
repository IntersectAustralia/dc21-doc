# Search API

## Search
Search can be performed via the API. Construct a post request with the required search criteria parameters, as per the details below.

Authentication is done by including your API token into post parameters. API tokens can only be used for API actions. You can obtain an API token by clicking on your email address, the "Settings" at the top right of the DC21 application.

### Request

* Verb: POST
* URL: https://\<base_url_for_your_server\>/data_files/api_search
* Mandatory POST parameters:
  * **auth_token** - authentication token
* Optional POST parameters:
  * **from_date** - This is "Date->From Date" in search box of WEB UI: `"from_date"=>"2013-01-01"`
  * **to_date** - This is "Date->To Date" in search box of WEB UI: `"to_date"=>"2013-01-02"`
  * **filename** - This is "Filename" in search box of WEB UI: `"filename"=>"test"`
  * **description** - This is "Description" in search box of WEB UI: `"description"=>"test"`
  * **file_id** - This is "File ID" in search box of WEB UI: `"file_id"=>"test"`
  * **id** - This is "ID" in search box of WEB UI: `"id"=>"26"`
  * **stati** - This is "Type" in search box of WEB UI: `"stati"=>["RAW", "CLEANSED"]`
  * **automation_stati** - This is "Automation Status" in search box of WEB UI, `"automation_stati"=>["COMPLETE", "WORKING"]`
  * **access_rights_types** - This is the "Access Rights Type" in the search box of the WEB UI: `"access_rights_types"=>["Open", "Conditional", "Restricted"]`
  * **file_formats** - This is "File Formats" in search box of WEB UI, `"file_formats"=>["TOA5", "Unknown", "audio/mpeg"]`
  * **published** - This is "Type->PACKAGE->Published" in search box of WEB UI: `"stati"=>["PACKAGE"], "published"=>["true"]`.
  * **unpublished** - This is "Type->PACKAGE->Published" in search box of WEB UI: `"stati"=>["PACKAGE"],  "unpublished"=>["true"]`.
  * **published_date** - This is "Type->PACKAGE->Published Date" in search box of WEB UI: `"stati"=>["PACKAGE"], "published_date"=>"2013-01-01"`
  * **tags** - This is "Tags" in search box of WEB UI: `"tags"=>["4", "5"]`
  * **labels** - This is "Labels" in search box of WEB UI, `"labels"=>["label_name_1", "label_name_2"]`
  * **creators** - This is "Creators" in search box of WEB UI, `"creators"=>["creator1@example.com", "creator2@example.com"]`
  * **contributors** - This is "Contributors" in search box of WEB UI, `"contributors"=>["contributor_name_1", "contributor_name_2"]`
  * **grant_numbers** - This is the "Grant Numbers" in search box of WEB UI, `"grant_numbers"=>["grant_number_1", "grant_number_2"]`
  * **related_websites** - This is the "Related Websites" in the search box of WEB UI, `"related_websites"=>["http://www.intersect.org.au"]`
  * **facilities** - This is "Facility" in search box of WEB UI, ask system administrator to get facility ids : `"facilities"=>["27"]`
  * **experiments** - This is "Facility" in search box of WEB UI, when one facility is clicked, experiments of this facility are selectable, ask system administrator to get experiment ids: `"experiments"=>["58", "54"]`
  * **variables** - This is "Columns" in search box of WEB UI, when one group is clicked, columns of this group are selectable: `"variables"=>["SoilTempProbe_Avg(1)", "SoilTempProbe_Avg(3)"]`
  * **uploader_id** - This is "Added By" in search box of WEB UI, ask system administrator to get uploader ids: `"uploader_id"=>"83"`
  * **upload_from_date** - This is "Date Added->From Date" in search box of WEB UI, `"upload_from_date"=>"2013-01-01"`
  * **upload_to_date** - This is "Date Added->To Date" in search box of WEB UI, `"upload_to_date"=>"2013-01-02"`


#### Regular expressions for Filename, Description and ID

You may search for data files using regular expressions. The DC21 application supports [POSIX Basic & Extended Regular Expressions](http://en.wikipedia.org/wiki/Regular_expression#POSIX_Basic_Regular_Expressions).

For example, to search for files with filenames starting with WTC, use `"filename"=>"^WTC"`.

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

url = "http://<host:port>/data_files/api_search" # Please change the host:port part!
params = {"stati"=>"RAW", "auth_token"=>"RQdasxNyCdzzZoNGfPvw"} # Generate your own token and paste here
response = RestClient.post(url, params)

puts response
```

Similar libraries exist in other languages (e.g. HttpClient for Java), plus there are various command line tools such as curl which could also be used.

Here is a example of unix curl command:

```bash
$ curl -X POST -d "stati=RAW&auth_token=RQdasxNyCdzzZoNGfPvw" http://<host:port>/data_files/api_search
```

The command line options used here:
```
-X, --request <command>
-d, --data <data>
-k, --insecure
```

Other options and arguments please refer to manpage of curl(1)

## Download

The response of the API Search will return a JSON object, where each data file that matched the criteria will contain a `url` value, eg. http://\<host\:port\>/data_files/21/download in 1.8.02 and http://\<host\:port\>/data_files/21/download.json in 1.9.01 onwards

### Request

* Verb: GET
* URL: \<url_from_json_response\>
* Mandatory GET parameters:
  * **auth_token** - authentication token

To download the file to the current directory you're in, use:
```bash
curl -OJLk <url_from_json_response>?auth_token=RQdasxNyCdzzZoNGfPvw
```

The command line options used here:
```
-O, --remote-name
-J, --remote-header-name
-L, --location
-k, --insecure
```

Other options and arguments please refer to manpage of curl(1)

Please note that the above command assumes that the authentication token is valid and is a basic way of saving a file using the command line. The download.json method will return a 401 error if the authentication fails and your script using the API will have to account for that.
