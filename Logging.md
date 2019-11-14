# Python
------
	self._logger.info("hallo welt)


## Logging from different classes 
Just pass the main-logger from your plugin class into your child classes (like DatabaseManager.py)

```python
	def __init__(self, parentLogger):
		self._logger = logging.getLogger(parentLogger.name + "." + self.__class__.__name__)
```
LogOuput:
```
2019-11-14 18:18:08,153 - octoprint.plugins.PrintJobHistory - INFO - Init everything
2019-11-14 18:18:08,153 - octoprint.plugins.PrintJobHistory.DatabaseManager - INFO - Creating new database in: /Users/o0632/.../printJobHistory.db

```


#JavaScript
----------
	console.log("Hallo welt");