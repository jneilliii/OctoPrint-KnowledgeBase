# Define Keys

`SETTINGS_KEY_START_PRINT_DELAY = "startPrintDelay"`

# Default Values
Also used for Reset-Function (see ...todo insert link ...)

	##~~ SettingsPlugin mixin
	def get_settings_defaults(self):
		return dict(
			# put your plugin's default settings here
                        startPrintDelay = 120,  # seconds
                        ....
		)



IMPORTEND: use custom binding!!!

    # ~~ TemplatePlugin mixin
    def get_template_configs(self):
        return [
            dict(type="settings", custom_bindings=True)
        ]


# Validate settings after save

```
def on_settings_save(self, data):
        layerExpressions = data.get(SETTINGS_KEY_LAYER_EXPRESSIONS)
        if layerExpressions is not None:
        ... do something usefull. eg. send message to browser ... 
```