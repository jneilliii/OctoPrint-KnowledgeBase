We have two options to send Data to the Plugin on the server
1. via command
2. via REST-Call

```javascript
$.ajax({
    url: API_BASEURL + "plugin/"+PLUGIN_ID_string+"?action=resetSettings",
    type: "GET"
}).done(function( data ){
    new PNotify({
        title: "Default settings saved!",
        text: "The plugin-settings were now reseted ...",
        type: "info",
        hide: true
    });
```

```python
class DisplayLayerProgressAPIPlugin(...octoprint.plugin.SimpleApiPlugin,...):

	def on_api_get(self, request):
		if len(request.values) != 0:
			action = request.values["action"]
```


# Simple REST API

```python
	class DisplayLayerProgressAPIPlugin(...octoprint.plugin.BlueprintPlugin,...):

	# GET http://localhost:5000/plugin/DisplayLayerProgressAPI/echo?text=123
	# -H X-Api-Key: 57FECA453FE94D46851EFC94BC9B5265
	@octoprint.plugin.BlueprintPlugin.route("/echo", methods=["GET"])
	def myEcho(self):
		if not "text" in flask.request.values:
			return flask.make_response("Expected a text to echo back.", 400)
		return flask.request.values["text"]
```