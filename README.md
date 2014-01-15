The DC21 application is a Ruby on Rails application built by [Intersect Australia](http://www.intersect.org.au/) for the [Hawkesbury Institute for the Environment]( http://www.uws.edu.au/hie/hie) as part of an [ANDS-Funded Data Capture Project - DC21 - Climate Change and Energy Research Facilities](http://projects.ands.org.au/id/DC21)

There's an additional Github repository at https://github.com/IntersectAustralia/restful-api-uploader/blob/2.0.01 which provides at set of scripts that can be run on a PC to automatically load files from that PC into DC21.

The latest stable version is:
* [DC21 2.0.01](https://github.com/IntersectAustralia/dc21/blob/2.0.01)
* [DC21 2.0.01 Documentation](https://github.com/IntersectAustralia/dc21-doc/blob/2.0.01/README.md)
* [Restful API uploader 2.0.01](https://github.com/IntersectAustralia/restful-api-uploader/blob/2.0.01)

Please feel free to browse any of the following documentation.
### For everyone:
* [Product Overview](Product_Overview.md)
* Cucumber Features - these are our automated acceptance tests, and they express (in plain English) the requirements of the system. This is a great place to start if you want to get an understanding of what the system does. There's a number of different ways to access them:
  * [Browse on Github](https://github.com/IntersectAustralia/dc21/blob/2.0.01/features).
  * Clone the [repository](https://github.com/IntersectAustralia/dc21/) and browse in your preferred tool.
* [Overview Diagram](Overview_Diagram.md)

### For deployers:
* [Deployment Guide](Deployment_Guide.md) - instructions on how to install the web application and do subsequent updates.
* [Setting Up Automated Load From PC](Setting_Up_Automated_Load_From_PC.md) - how to automatically send files to DC21 from a PC
* [Loading external template files](Loading_External_Template_Files.md) - how to tell the web application to read an external template file for README.html without redeploying
* [Upgrading From 1.9.04 to 2.0.01](Upgrading_From_1.9.04_to_2.0.01.md) - Instructions to upgrade your server from 1.9.04.
###  For developers:
* [Developers Guide](Developers_Guide.md) - useful information for future developers of the application.
  * [Development Environment Setup](Development_Environment_Setup.md) - instructions for setting up a dev environment.
* [Domain Model](Domain_Model.md) - overview of the domain concepts in the system.
* [RIFCS Mapping](RIFCS_Mapping.md) - details of the RIFCS records produced by the system.
* [File Upload API](File_Upload_API.md) - details of the API for automatically uploading/downloading files
* [Search API](Search_API.md) - details of the API for scripting search
* RSpec tests - the RSpec examples express the detailed requirements so are a good place to start getting an understanding of what the system does.  There's a number of different ways to access them:
  * [Browse on Github](https://github.com/IntersectAustralia/dc21/blob/2.0.01/spec).
  * Clone the [repository](https://github.com/IntersectAustralia/dc21) and browse in your preferred tool.

### Version Documentation:
The version documentation contains the release notes and the user manual.
* Download Version 1.9.04 here : [DC21v1.9.04DocFiles.zip](files/HIEv_v1.9.04DocFiles.zip?raw=true)
* Download Version 1.8.02 here : [DC21v1.8.02DocFiles.zip](files/HIEvv1.8.02DocFiles.zip?raw=true)
* Download Version 1.6b.03 here : [DC21v1.6b.03DocFiles.zip](files/DC21v1.6b.03DocFiles.zip?raw=true)
