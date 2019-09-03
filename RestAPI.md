# How to implement a simple REST API?

	class DisplayLayerProgressAPIPlugin(...octoprint.plugin.BlueprintPlugin,...):

	# GET http://localhost:5000/plugin/DisplayLayerProgressAPI/echo?text=123
	# -H X-Api-Key: 57FECA453FE94D46851EFC94BC9B5265
	@octoprint.plugin.BlueprintPlugin.route("/echo", methods=["GET"])
	def myEcho(self):
		if not "text" in flask.request.values:
			return flask.make_response("Expected a text to echo back.", 400)
		return flask.request.values["text"]