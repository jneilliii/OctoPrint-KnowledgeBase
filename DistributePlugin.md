https://plugins.octoprint.org/help/registering/

Short
* Fork of https://github.com/OctoPrint/plugins.octoprint.org
* Checkout to your local filesystem

Message: ```This branch is X commits behind OctoPrint:gh-pages```

```
Only once
git remote add upstream https://github.com/OctoPrint/plugins.octoprint.org

git fetch upstream
git checkout gh-pages
git reset --hard upstream/gh-pages
git push --force
```
Message: ```This branch is even with OctoPrint:gh-pages```

* Modify/Add new Plugin-Description
e.g. ```/plugins.octoprint.org/_plugins/DeleteAfterPrint.md```

* Build/Test your plugin description with 
```bundle exec jekyll serve```

* Create Merge Request
