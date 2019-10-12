# Create a new Plugin
http://docs.octoprint.org/en/master/plugins/gettingstarted.html

https://github.com/foosel/OctoPrint/wiki/Setup-on-Windows

Preconditions: 
- Octoprint is installed
- Cookiecutter installed 
	pip install "cookiecutter>=1.4,<1.7" 
```
        cd Octoprint-latest/
	virtualenv venv
        source venv/bin/activate

        octoprint dev plugin:new DryRun
	plugin_package [octoprint_DryRun]:
	plugin_name [OctoPrint-Deleteafterprint]: DryRun
	repo_name [OctoPrint-DeleteAfterPrint]:OctoPrint-DryRun
	full_name [You]: OllisGit
	email [you@example.com]:
	github_username [you]: OllisGit
	plugin_version [0.1.0]: 1.0.0
	plugin_description [TODO]: Execute printing without heating and extrusion
	plugin_license [AGPLv3]:
	plugin_homepage [https://github.com/OllisGit/OctoPrint-DeleteAfterPrint]:
	plugin_source [https://github.com/OllisGit/OctoPrint-DeleteAfterPrint]:
	plugin_installurl [https://github.com/OllisGit/OctoPrint-DeleteAfterPrint/archive/master.zip]:
```
* The new plugin folder structure is created in the current folder. Move it to where ever you like.

* Create new GitHub Repository via Web-UI. Use the same name as you used during the creation.

* Switch to your new plugin folder and init/push to the git repository:
```
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/OllisGit/OctoPrint-SpoolManager.git
git push -u origin master
```
