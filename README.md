# wp-convert-acf-php-to-json
Convert Wordpress ACF fields registered by PHP to json

Source: https://dev-notes.eu/2017/01/convert-acf-fields-registered-by-php-to-importable-json-format/
Credits to David Egan

## USE CASE

In older projects we registered ACF fields by importing to PHP. This is good for the deployed site - the fields load quicker, and users can’t amend the fields.

This can be problematic if you import a production database into a local development environment. The fields are not registered in the production database. When you import it, you no longer have access to the fields in the ACF GUI. The fields still exist (they are registered by PHP functions), but you can’t use the nifty ACF interface to edit them.

Because of this we have switched to ACFs local JSON option. ACF writes the field registration data in JSON format to a server-writable directory (`acf-json`, unless you override the default) in the theme root. You end up with a separate JSON file for each field group. The JSON configuration files can go into your version control system. You can deploy to staging or production, and if you accidentally delete files in your dev environment (for example, if you import a database) you can easily re-sync the JSON files to have them show up in the ACF GUI again.

If you have your fields registered by PHP (not in the database), how do you switch to the local JSON workflow?

1. Create a JSON configuration file that can be read by the ACF “Import” function - i.e. mimic the output of the ACF “Export” function
2. Check the permissions `acf-php-export.json` in your theme root and run `sudo chown www-data acf-php-export.json`
3. Import the JSON file to ACF

## CREATING THE JSON FILE FOR IMPORT

The following quick and dirty method is suitable for a local development environment:

1. Clone the repo to the theme folder, or somewhere in wordpress. 
2. Make sure the path is updated on line 3. `require('../../../../wp-load.php');`
2. Set permissions to the default file that will export the php to json and make it server-writable `sudo chown www-data acf-php-export.json`
3. Hit the page in your browser - you’ll see the JSON data output, and this will be written to the config file
4. Import the config file: within the ACF “Tools” > “Import Field Groups” interface, select the file and click import

The field configurations are now held in the local database, and will be saved in the `acf-php-export.json` file.