### DIVER
This is the documentation repository for the DIVER software application. DIVER consists of a number of source code repositories, the primary repo is [dc21](https://github.com/IntersectAustralia/dc21). Respoisoties associated with each version of DIVER are listed  in the Version table below.

### About DIVER
DIVER (Data Is Vital for Empirical Research) is a general purpose, user-friendly research data capture and sharing application. DIVER was originally developed by [Intersect Australia](http://www.intersect.org.au/) for the [Hawkesbury Institute for the Environment](http://www.westernsydney.edu.au/hie) at the [Western Sydney Unversity](http://www.westernsydney.edu.au/) as part of [ANDS](http://www.ands.org.au/) Data Capture Project [DC21] (https://projects.ands.org.au/id/DC21) in 2012/2013.

DIVER  is a Ruby on Rails (3.2) application for the management of environmental data, predominantly time-series from Campbell Scientific Loggers, but also for NETCDF, NCML format data and any general purpose data.

### Versions
The following provides links to the respective repositories for each version.
The version documents contain the release notes and the user manual.
Please feel free to browse any of the following documentation.

| Version | Date | Repo Links |  Version Documents |
| --- | :-- | :-- | :-- | :-- | :-- |
| 2.1.02 | Jun 2014 | [App](https://github.com/IntersectAustralia/dc21/tree/2.1.02) <br> [ Doc](https://github.com/IntersectAustralia/dc21-doc/tree/2.1.02/README.md)  <br> [RESTful API uploader](https://github.com/IntersectAustralia/restful-api-uploader/tree/2.1.02)  <br> [EyeTracker Packager](https://github.com/IntersectAustralia/dc21-eyetracker-packager/tree/2.1.02) | [DC21 v2.1.02 ReleaseNotes.pdf](files/DC21 v2.1.02 ReleaseNotes.pdf?raw=true) <br> [DC21 v2.1.02 UserManual.pdf](files/DC21 v2.1.02 UserManual.pdf?raw=true)|
| 2.0.01 | Dec 2013 | [App](https://github.com/IntersectAustralia/dc21/tree/2.0.01) <br> [Doc](https://github.com/IntersectAustralia/dc21-doc/tree/2.0.01/README.md)  <br> [RESTful API uploader](https://github.com/IntersectAustralia/restful-api-uploader/tree/2.0.01)  <br> [EyeTracker Packager](https://github.com/IntersectAustralia/dc21-eyetracker-packager/tree/2.0.01) | [DC21_v2.0.01_DocFiles.zip](files/DC21_v2.0.01_DocFiles.zip?raw=true) |
| 1.9.04 | Mar 2012 | [App](https://github.com/IntersectAustralia/dc21/tree/1.9.04) <br> [ Doc](https://github.com/IntersectAustralia/dc21-doc/tree/1.9.04/README.md)  <br> [RESTful API uploader](https://github.com/IntersectAustralia/restful-api-uploader/tree/1.9.04)  <br> | [DC21v1.9.04DocFiles.zip](files/HIEv_v1.9.04DocFiles.zip?raw=true)


### For everyone:
* [Product Overview](Product_Overview.md)
* Cucumber Features - these are our automated acceptance tests, and they express (in plain English) the requirements of the system. This is a great place to start if you want to get an understanding of what the system does. There's a number of different ways to access them:
  * [Browse on Github](https://github.com/IntersectAustralia/dc21/tree/2.0.01/features).
  * Clone the [repository](https://github.com/IntersectAustralia/dc21/) and browse in your preferred tool.
* [Overview Diagram](Overview_Diagram.md)

### For deployers:
* [Deployment Guide](Deployment_Guide.md) - instructions on how to install the web application and do subsequent updates.
* [Setting Up Automated Load From PC](Setting_Up_Automated_Load_From_PC.md) - how to automatically send files to DC21 from a PC
* [Loading external template files](Loading_External_Template_Files.md) - how to tell the web application to read an external template file for README.html without redeploying

###  For developers:
* [Developers Guide](Developers_Guide.md) - useful information for future developers of the application.
  * [Development Environment Setup](Development_Environment_Setup.md) - instructions for setting up a dev environment.
* [Domain Model](Domain_Model.md) - overview of the domain concepts in the system.
* [RIFCS Mapping](RIFCS_Mapping.md) - details of the RIFCS records produced by the system.
* [File Upload API](File_Upload_API.md) - details of the API for automatically uploading/downloading files
* [File Update API](File_Update_API.md) - details of the API for updating metadata of data files
* [Search API](Search_API.md) - details of the API for scripting search
* [Package Create API](Package_Create_API.md) - details of the API for creating packages
* [Package Publish API](Package_Publish_API.md) - details of the API for publishing packages
* RSpec tests - the RSpec examples express the detailed requirements so are a good place to start getting an understanding of what the system does.  There's a number of different ways to access them:
  * [Browse on Github](https://github.com/IntersectAustralia/dc21/tree/2.0.01/spec).
  * Clone the [repository](https://github.com/IntersectAustralia/dc21) and browse in your preferred tool.
