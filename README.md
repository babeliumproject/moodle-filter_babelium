[Babelium Assignment Plugin]: http://github.com/babelium/moodle-assignment_babelium
[Babelium]: http://babeliumproject.com
[Request a set of API keys]: http://babeliumproject.com/moodleapi.php

#Babelium Filter for Moodle
Babelium is an open source video platform aimed at second language learning. Language learners are able to record their voice using a browser and the cuepoint-constrained videos that are freely available at our main website.

These instructions describe how to install the Babelium Filter for Moodle. This plugin adds a settings page to your Moodle platform, enabling your admin to change all the data and options required by the Babelium Assignment in an easy way. This plugin is a companion plugin for the [Babelium Assignment Plugin][].

**Table of contents**
- [Obtaining the source](#obtaining-the-source)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Uninstall the plugin](#uninstall-the-plugin)

##Obtaining the source
To run the latest version of the filter plugin, clone the git repository.

	$ git clone git://github.com/babeliumproject/moodle-filter_babelium.git moodle-filter_babelium

Now the entire project should be in the `moodle-filter_babelium/` directory.

##Prerequisites

* Server running Moodle 2.0
* Babelium server (running on a separate machine)
* Web browser with Flash Player 11.1+
* Broadband connection (1Mbps/512Kbps)

We provide you a demo Babelium server at [Babelium][] for testing this plugin. To use this server you need to:
	
1. Sign Up for a free account
2. [Request a set of API keys][] for your Moodle server's domain (you will also need the account name you created in the previous step)

##Installation
The filter adds a new settings panel in (Site Administration -&gt; Modules -&gt; Filter -&gt; Manage Filters -&gt; Babelium) that allows your admin to configure the plugin without having access to the server's filesystem:

```sh
$ mkdir <moodle_directory>/filter/babelium
$ cd moodle-filter_babelium
$ cp -r * <moodle_directory>/filter/babelium
```

After copying the files, log in with an admin account into your Moodle site. Moodle should automatically detect that a new plugin is being added and prompt you for actions to take. If this is not the case, browse to the following URL to force the plugin installation page to appear:

	http://<moodle_domain>/admin

The filter settings page will automatically appear after the installation is done. Fill the settings with the information you got when registering your Moodle site in [Babelium] and click on **Save changes**. You can change the settings of the filter by visiting `Site Administration -> Modules -> Filters -> Manage Filters -> Babelium` using an admin account.


##Uninstall the plugin
This is useful when the plugin isn't correctly installed. With these steps you can completely remove the Babelium plugin, in order to do a fresh installation.

Remove all the plugin files and folders (they should be at this location):

```sh
rm -r <moodle_home>/filter/babelium
```

Then, in your Moodle database, clean the `mdl_config` table of any Babelium filter plugin values:

```sql
DELETE FROM mdl_config WHERE name LIKE 'filter_babelium%';
```

If `mdl_config` has a row called `textfilters`, remove the part of the field that belongs to Babelium (the values are comma-separated):

```sql
SELECT value FROM mdl_config WHERE name='textfilters' AND value LIKE '%babelium%';
```

The `mdl_filter_active` and `mdl_filter_config` tables might contain entries related to Babelium. If you find any, delete them:

```sql
DELETE FROM mdl_filter_active WHERE filter LIKE '%babelium%';
DELETE FROM mdl_filter_config WHERE filter LIKE '%babelium%';
```

Let's also clean any trace left in the `mdl_upgrade_log`:

```sql
DELETE FROM mdl_upgrade_log WHERE plugin='assignment_babelium';
```

With that, you should have removed all the traces of the failed plugin install. Please refer to the previous sections to do a fresh install of the plugin.
