# Offical description
https://plugins.octoprint.org/help/registering/

# Short description
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
```
Only once: 
bundle

bundle exec jekyll serve

http://127.0.0.1:4000/
```

* Commit/Push changes to your repository

* Create Merge Request
