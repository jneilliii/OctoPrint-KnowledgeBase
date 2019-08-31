# Create a new Plugin
http://docs.octoprint.org/en/master/plugins/gettingstarted.html

https://github.com/foosel/OctoPrint/wiki/Setup-on-Windows

Preconditions: 
- Octoprint is installed
- Cookiecutter installed 
	pip install "cookiecutter>=1.4,<1.7" 

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

