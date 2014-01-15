# Loading External Template Files

## To use external template files
To use another template file, in
`dc21app_config.yml` add
```
readme_template_file: readme_template.html.haml
readme_template_directory: /data/dc21-templates
```

Then in `/data/dc21-templates/` you create your HTML template. It will only read the filename defined in `dc21app_config.yml` - any other will be ignored. These values can be edited to point to anywhere but you will need to restart the web server for that change to take effect.

Note: Ensure that the directory has permission for the web application to write to.

## Reverting back to default template file
To revert back to the default template file you just simple rename or remove the file defined in `dc21app_config.yml`.

### Example
In our configuration file `dc21app_config.yml`
```
readme_template_file: readme_template.html.haml
readme_template_directory: /data/dc21-templates
```
Then the file path is `/data/dc21-templates/readme_template.html.haml`

## My file is not being read!
There are many different scenarios as to why it is not being used.
### 1 - File name mismatch
The name of defined under `readme_template_file` does not match to the filename

### 2 - Non-existant file
The actual file doesn't exist under the directory

### 3 - Non-existant directory
You should manually create the directory and give it the proper read/write permissions. The system does check and create if the directory doesn't exist (as defined at `readme_template_directory`). If it doesn't create the directory then it probably doesn't have permission to do so.
