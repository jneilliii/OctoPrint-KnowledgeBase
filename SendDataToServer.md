We have two options to send Data to the Plugin on the server
1. via action-command
2. via REST-Call

# Action Command
## GET - Call
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

## POST - Call
If you want to send more data to the server (e.g. settings) then you need to send the data via POST
and you need to declare the send action-command on the server side ```def get_api_commands(self)```

```javascript
            $.ajax({
                url: API_BASEURL + "plugin/"+PLUGIN_ID,
                type: "POST",
                dataType: "json",
                data: JSON.stringify({
                    command: "checkboxStates",
                    deleteWhenPrintCanceled: checkedDeleteCanceled
                }),
                contentType: "application/json; charset=UTF-8"
            })
```

```python
    def get_api_commands(self):
        return dict(checkboxStates=[])

    def on_api_command(self, command, data):
        #if not user_permission.can():
        #    return make_response("Insufficient rights", 403)

        if command == "checkboxStates":
            if data.has_key("deleteAfterPrint"):
                self._deleteAfterPrintEnabled = bool(data["deleteAfterPrint"])
            if  data.has_key("deleteWhenPrintCanceled"):
                self._deleteWhenPrintCanceled = bool(data["deleteWhenPrintCanceled"])

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