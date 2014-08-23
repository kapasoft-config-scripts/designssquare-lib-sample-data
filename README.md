Sample Data - to automate import/export sample data for Drupal Themes & Widgets
================================
```html
This is an overview of the custom module - <a href="https://www.drupal.org/sandbox/kapacs/2287423" title="Sample Data">Sample Data</a> developed and used at <a href="http://designssquare.com" title="Drupal widgets and themes">DesignsSquare.com</a> that was published at Drupal.org for anyone that finds it useful

<b>Issue:</b> Widget or Theme needs sample data for kickstart. There is no automatic, easy and standard way to package Sample Data with all of its assets(i.e. images, videos,etc) part of single artifact deliverable for easy install in Drupal.

<b>Solution:</b> Use Features module along with Sample Data module for sample data exports/imports in an automotive manner without another dependency

The common practice to package widgets and themes at DesignsSquare.com delivered to client is by separating the deliverable into 3 parts - code, Structures&Configurations and Sample Data for KickStart all part of one deliverable via Features. The Sample Data part contains sample nodes, menu instances and assets referenced from content, fields and variables.

To automate the packaging process for Sample Data part of the deliverable, we have created Sample Data module. The Sample Data module does the following:
<ul>
<ol> <strong>1.</strong> Export/Imports nodes with Alias path </ol>
<ol> <strong>2.</strong> Export/Imports menus based on Alias path </ol>
<ol> <strong>3.</strong> Handle the assets – images, videos, etc for sample data. Specifically:
    <ul>
	<ol> <strong>A)</strong> assets referenced from sample content or other modules located in default public directory(i.e. sites/default/file)</ol>
        <ol> <strong>B)</strong> assets referenced by fields(both core type or custom type)</ol>
        <ol> <strong>C) </strong>assets referenced by variables </ol>
   </ul>
</ol>
<ol><strong>4.</strong> Overrides for context and StormArm variables</ol>
</ul>


Note: assets referenced by custom fields and variables need to use file_managed functionality to work

<h3>1. Export/Imports nodes with Alias path</h3>
In order to have alias path, you will have to install and enable alias path module - <a href="https://www.drupal.org/project/pathauto" title="pathauto">https://www.drupal.org/project/pathauto</a>. Once the sample data have alias path, then export it from features(admin/structure/features). The Sample Data module will hook into the export and store alias path for each node later to restore at the time of import

<h3>2. Export/Imports Menus based on Alias path</h3>
Once you have the pathauto module enabled, each node will have the alias path used by Sample Data to export and import Menus. Go to Features(admin/structure/features) and look for section "MENU ALIAS" with options to select menues you like to import/export. It will store the menus in the export and then import to the new instance based on alias path. Since it uses alias path, each menu has to have unique links(alias path) to work

<h3>3A) Assets referenced from sample content or other modules</h3>
Your sample content may be referencing assets.  It is also possible that other modules like "imce" for editor is storing assets. The Sample Data module lets you export those assets. Go to Features(admin/structure/features) and look for section "Content Assets". In this section, it will display all assets(files and directories) from the public directory. Select the ones you like to export and it will store them for later import in the new instance. If you select directory, then it will store the whole tree of the assets

<h3>3B) Assets Referenced by Fields</h3>
Your sample data(nodes) may have fields of type 'file' that is going to reference assets. The Sample Data module will automatically store those assets when you are exporting the sample data(nodes). It will automatically transfer the assets at the time of import in the new instance. The Sample Data module does the same for custom fields referencing assets and not only the Drupal default such as 'file'. There is one requirement. The 'file_managed' functionality needs to be handling the asset in order for the import/export to work

<h3>3C) Assets Referenced By Variables </h3>
You may have chosen to store asset references in form of FID in the Drupal variable. The Sample Data module will export/import this assets as long as the name of the variable is stored part of the entry in the file_usage table for field(column) - 'type':
[code]
 //we also add variable name as the 'type' parameter, so we can export via Sample Data module
        file_usage_add($file, 'module_name', 'VARIABLE_NAME', '1');
        // Save.
        file_save($file);
[/code]
As you see, the variable name is saved part of the usage entry. This is done, so Sample Data module exports the asset

Once the variable is holding reference to the asset and the variable name is stored part of entry in the file_usage table, then in section 'VARIABLE ASSETS' in the Features page(admin/structure/features) there is listed available references for you to select to export/import those assets.

<h3>4. Overrides for context and StormArm variables</h3>
The "CONTEXT OVERRIDE" provides a solution to the fact that features doesn't let you export something that is already part of another feature. This is an issue if you say have a widget with context that needs to be overridden by site specific context. So by select the context from "CONTEXT OVERRIDE", it will store those context to import in the new instance

The "STRONGARM OVERRIDE", besides the ability of overriding another conflicting feature, it also provides a solution to be able export home page, error 404, 403 that is impossible otherwise because it uses the hard coded path(node/id) specific to each Drupal instance. The Sample Data utilizes alias path to import/export home page, error 404 and 403 pages

To summarize, the Sample Data module is one full solution for exporting/importing Sample data for Kickstart data. It exports/imports nodes and menus using alias path. It handles all the assets referenced by content, fields or variables. It also provides ability to override contexts as well as export/import home page, error 404, 403 pages. It does all for you, so you can focus on more important aspects than handling sample data/assets when building widgets,themes and other cool things

<h3>Troubleshooting</h3>
<h5>I am unable to export Content(nodes) via futures</h5>
Ensure you have the correct versions for features(7.x-2.0 or above), feature_uuid(7.x-1.0-alpha4...the dev version on July 2014) and uuid(7.x-1.0-alpha5...dev version on July, 2014). Afterwards, enable the content to by exportable at admin/config/content/uuid_features

<h5>The specified file public://artifact-name.jpg could not be copied, because the destination directory is not properly configured</h5>
Ensure the export feature module has write access and recreate the feature.