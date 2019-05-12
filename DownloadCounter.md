Example could be found here: https://github.com/OllisGit/OctoPrint-CICD_Playground



The solution is based on the following steps

1. Create badge-image: done via [http://shields.io](http://shields.io/)
2. Provide endpoint for download statistics: done via github rest api
3. Create downloadable plugin artifact (a.k.a release asset): manually or via https://travis-ci.org/
4. Change the pip-download url in your plugin

1 ) Shield.io provides pre-defined badges, static or dynamic image creation.
I use the following urls (see `README.md` in example repo for markdown) 

Pre-defined for total downloads:
`https://img.shields.io/github/downloads/OllisGit/OctoPrint-CICD_Playground/latest/total.svg`

Two dynamic images for latest version-name and release-date:
```
https://img.shields.io/badge/dynamic/json.svg?color=brightgreen&label=version&url=https://api.github.com/repos/OllisGit/OctoPrint-CICD_Playground/releases&query=$[0].name

https://img.shields.io/badge/dynamic/json.svg?color=brightgreen&label=released&url=https://api.github.com/repos/OllisGit/OctoPrint-CICD_Playground/releases&query=$[0].published_at
```
You need the following information: github api-endpoint and a json path query to select the correct information.
```
url: https://api.github.com/repos/OllisGit/OctoPrint-CICD_Playground/releases
query: $[0].published_at
```
Description how to use this could be found here: https://shields.io/#dynamic-badge

2 ) The GitHub API (https://developer.github.com/v3/repos/releases/) provides several information about the releases. You need to filter the response via JSON-Path and put it into the image-url (see above) 

BUT the biggest pain in the ... is, that the response only provides download-counts for assets and not for the source zip file (<repo>/archive/{target_version}.zip is not counted).
So, it is necessary to create an release asset zip-file of the current sourcecode.

3 ) You can do this manually or by travis-ci.org. 
In my case the travis build is triggerd when I push something to the repo. The build creates an zip-file of the current master branch, creates a pre-release and attach the zip file as “master.zip” to the pre-release as an asset.

It was a little bit tricky to update an release, but thank google this problem could be solved. 
If you need more info about the build-job just let me know or look into the example.

4 ) Normally the pip upload url looks like this: 
`pip="https://github.com/OllisGit/OctoPrint-CICD_Playground/archive/{target_version}.zip`
You need to change it to:
```
##~~ Softwareupdate hook
def get_update_information(self):
...
pip="https://github.com/OllisGit/OctoPrint-CICD_Playground/releases/latest/download/master.zip
```
Now, if you push (merge) some data (e.g. setup.py with new version-number) travis execute the build and creates a pre-release. Octoprint didn’t recognizes this, because it is a pre-release (default-behavior).

You need to create the release manual, by editing the pre-release via the github ui. 
After you released the version, Octoprint detects the new version, download/install it and the download badge counter on the plugin homepage is increased (could take 1-3 minutes).

What do you think?