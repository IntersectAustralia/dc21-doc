# Setting Up Automated Load From PC

This guide is windows-specific. Other operating systems will be similar, but with different paths.

## Setup

### Download and unzip scripts
Download the [API load scripts from Github](https://github.com/IntersectAustralia/restful-api-uploader/zipball/1.9.04)

Unzip the downloaded package into a directory of your choice. For these instructions we'll assume you've unzipped it to
```
C:\scripts\dc21\
```
and thus have a directory something like
```
C:\scripts\dc21\IntersectAustralia-restful-api-uploader-5c265ec
```

### Run setup scripts
In windows explorer, find the directory you just unzipped to and double-click the windows_setup_1.bat file followed by windows_setup_2.bat.
```
C:\scripts\dc21\IntersectAustralia-restful-api-uploader-5c265ec\windows_setup_1.bat
C:\scripts\dc21\IntersectAustralia-restful-api-uploader-5c265ec\windows_setup_2.bat
```

### Edit config file
There should now be two configuration files in your base directory e.g.
```
C:\scripts\dc21\wrapper_config.yml
C:\scripts\dc21\transfer_config.yml
```
Edit these directories to contain your desired values. Both configuration files should contain reference to a similar list of 'files' for upload. It is best to edit these files with notepad or notepadd ++, rather than wordpad, as the formatting of spaces and tabs is important. Both files contain comments instructing you how to fill them in. You will need to concern yourself with:

transfer_config.yml -------------------------------------------
* **api_endpoint**: modify the base URL, but leave the end section as /data_files/api_create
* **auth_token**: put your own API token here, you can obtain this in the user settings page of DC21
* **files**: this section needs to contain one stanza per file you wish to upload
  * **path**: absolute path of the file
  * **type**: (required) e.g. RAW, PROCESSED (must be one of the recognised types)
  * **experiment_id** (or **org_level2_id**): (required) numeric id of the experiment (or organisational level 2 entry), you can obtain this from the experiment (organisational level 2 entry) details page of DC21
  * **description**: (optional) description of the file
  * **tags**: (optional) tags to be applied, must be from the legal set of tags, and must be comma separated and placed in single quotes - e.g. 'Photo,Video'
  * **backup**: (optional) absolute path of *existing* Backup folder.
  * **parent_filenames**: (optional) parent files for the file must exist on the server prior to upload, and must be an array of file names
  * **label_names**: (optional) labels to be applied, which must be comma separated and placed in single quotes

wrapper_config.yml -------------------------------------------
* **files**: this section needs to contain one stanza per file you wish to upload (these should be the same files as specified in transfer_config.yml)
  * **path**: (required) absolute path to the directory for this file
  * **file**: (required) file name (including extension)
  * **destination**: (at least one required) The absolute paths used for transfer and backup of files. The first destination is taken to be the 'upload pending' directory, and should match the path specified for the file in transfer_config.yml. Any further destinations will be taken as 'backup' locations and the file will be copied to these destiantions each time the script is executed.
  * **rotation**: (optional) the backup parameter. Rotation can be specified as daily, weekly or monthly. This is used to calculate a 'rotation date' for file backups. Upon each rotation date (at the end of each day, week or month, respectively), the source file will be moved to its destination(s) and removed from its source location, as opposed to non-rotation dates in which the file is simply copied to its destination. The resulting name of each copied/moved file will include the date of its next rotation.


### Test run
Run the batch file windows_api_load.bat. This can be found at
```
C:\scripts\dc21\windows_api_load.bat
```

Results of the test run will be written to
```
"C:\scripts\dc21\IntersectAustralia-restful-api-uploader-5c265ec\log\log.txt"
```
### Scheduling
Windows Task Scheduler can be used to schedule the load to run at regular times.
* Scheduled Tasks (usually under Accessories, System Tools)
* Add Scheduled Task
* When prompted to select the program to run, click "Browse.." and select the batch file you tested earlier:
```
C:\scripts\dc21\windows_api_load.bat
```
* Select appropriate timing, e.g. daily
* Select appropriate time and start date
* Enter your username and password if prompted
* Click Finish

## Ongoing Maintenance

### Clearing the log
If you wish to clear old log entries, simply delete the log file. It will be re-created on the next run.

### Modifying the config
In future you can simply modify the config files, and the changes will be automatically picked up on the next run.
