### DIVER
This is the documentation repository for the DIVER automated research data captures application. DIVER consists of a number of source code repositories, the primary repo is [dc21](https://github.com/IntersectAustralia/dc21).

### About DIVER
DIVER (Data Is Vital for Empirical Research) is a research data capture and sharing application. DIVER has extensive APIs and tools that enable automated data capture, analysis and publishing. DIVER was originally developed by [Intersect Australia](http://www.intersect.org.au/) for the [Hawkesbury Institute for the Environment](http://www.westernsydney.edu.au/hie) at the [Western Sydney Unversity](http://www.westernsydney.edu.au/) as part of [ANDS](http://www.ands.org.au/) Data Capture Project [DC21] (https://projects.ands.org.au/id/DC21) in 2012/2013.

DIVER  is a Ruby on Rails (3.2) application for the management of environmental data, predominantly time-series from Campbell Scientific Loggers, but also for NETCDF, NCML format data and any general purpose data.

### Launching an instance of DIVER on the NeCTAR Research Cloud
[Here](http://www.intersect.org.au/content/launchpod) is a description of how to launch a trial instance of DIVER on the NeCTAR research cloud using Intersect's Launchpod.

### Licence Information
The DIVER code is licensed under the GNU GPL v3 license - see [LICENSE.txt](https://github.com/IntersectAustralia/dc21/blob/master/LICENSE.txt).

### Versions
Contact Intersect to determine which versions are currently supported.

| Version | Date | Repo Links |  Version Documents | Summary |
| --- | :-- | :-- | :-- | :-- | :-- | :-- |
| 2.2 | Jun 2015 | [App](https://github.com/IntersectAustralia/dc21/tree/2.2), [ Doc](https://github.com/IntersectAustralia/dc21-doc/tree/2.2/README.md), [RESTful API uploader](https://github.com/IntersectAustralia/restful-api-uploader/tree/2.1.02), [EyeTracker Packager](https://github.com/IntersectAustralia/dc21-eyetracker-packager/tree/2.1.02), [Python Wrapper](https://github.com/IntersectAustralia/divermodc) | [ReleaseNotes.pdf](files/DC21 v2.1.02 ReleaseNotes.pdf?raw=true), [UserManual.pdf](files/DC21 v2.1.02 UserManual.pdf?raw=true)| NETCDF support and Python Wrapper
| 2.1.02 | Jun 2014 | [App](https://github.com/IntersectAustralia/dc21/tree/2.1.02), [ Doc](https://github.com/IntersectAustralia/dc21-doc/tree/2.1.02/README.md), [RESTful API uploader](https://github.com/IntersectAustralia/restful-api-uploader/tree/2.1.02), [EyeTracker Packager](https://github.com/IntersectAustralia/dc21-eyetracker-packager/tree/2.1.02) | [ReleaseNotes.pdf](files/DC21 v2.1.02 ReleaseNotes.pdf?raw=true), [UserManual.pdf](files/DC21 v2.1.02 UserManual.pdf?raw=true)| Access Control |
| 2.0.01 | Dec 2013 | [App](https://github.com/IntersectAustralia/dc21/tree/2.0.01),  [Doc](https://github.com/IntersectAustralia/dc21-doc/tree/2.0.01/README.md),  [RESTful API uploader](https://github.com/IntersectAustralia/restful-api-uploader/tree/2.0.01), [EyeTracker Packager](https://github.com/IntersectAustralia/dc21-eyetracker-packager/tree/2.0.01) | [DocFiles.zip](files/DC21_v2.0.01_DocFiles.zip?raw=true) | Org Structure and Tags |
| 1.9.04 | Mar 2012 | [App](https://github.com/IntersectAustralia/dc21/tree/1.9.04), [ Doc](https://github.com/IntersectAustralia/dc21-doc/tree/1.9.04/README.md), [RESTful API uploader](https://github.com/IntersectAustralia/restful-api-uploader/tree/1.9.04), | [DocFiles.zip](files/HIEv_v1.9.04DocFiles.zip?raw=true) | Packaging and Publishing


### For everyone:
* [Product Overview](Product_Overview.md)
* Cucumber Features - these are automated acceptance tests that express (in plain English) the requirements of the system. Apart from the User Manual and Release Notes, this is a great place to start if you want to get an understanding of what the system does. There's a number of different ways to access them:
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
* [File Update API](File_Update_API.md) - details of the API for updating metadata of data files (version 2.3 onwards)
* [Org Structure List API](Org_Structure_List_API.md) - details of the API to extract the DIVER organisational structure (version 2.2 onwards)
* [Search API](Search_API.md) - details of the API for scripting search
* [Package Create API](Package_Create_API.md) - details of the API for creating packages (version 2.3 onwards)
* [Package Publish API](Package_Publish_API.md) - details of the API for publishing packages (version 2.3 onwards)
* [Variable List API](Variable_List_API.md) - details of the API to extract DIVER column mappings and variables (version 2.2 onwards)
* RSpec tests - the RSpec examples express the detailed requirements so are a good place to start getting an understanding of what the system does.  There's a number of different ways to access them:
  * [Browse on Github](https://github.com/IntersectAustralia/dc21/tree/2.0.01/spec).
  * Clone the [repository](https://github.com/IntersectAustralia/dc21) and browse in your preferred tool.

### Acknowledgements
DIVER software development has been funded by the Australian National Data Service (ANDS, http://ands.org.au), Western Sydney University, Macquarie University and Intersect. ANDS is supported by the Australian Government through the National Collaborative Research Infrastructure Strategy Program (NCRIS) and the Education Investment Fund (EIF) Super Science Initiative.
