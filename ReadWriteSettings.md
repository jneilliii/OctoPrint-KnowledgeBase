# Python

## Define Keys

`SETTINGS_KEY_START_PRINT_DELAY = "startPrintDelay"`

## Get/Set Values
```
has
get
get_int
get_float
get_boolean
set
set_int
set_float
set_boolean
remove

Example: self._settings.set(["mySettingsKey"], myValue)
```

## Default Values
Also used for Reset-Function (see ...todo insert link ...)

	##~~ SettingsPlugin mixin
	def get_settings_defaults(self):
		return dict(
			# put your plugin's default settings here
                        startPrintDelay = 120,  # seconds
                        ....
		)
        # DON'T FORGET: default save function
        octoprint.plugin.SettingsPlugin.on_settings_save(self, data)


IMPORTEND: use custom binding!!!

    # ~~ TemplatePlugin mixin
    def get_template_configs(self):
        return [
            dict(type="settings", custom_bindings=True)
        ]


## Validate settings after save

```
def on_settings_save(self, data):
        layerExpressions = data.get(SETTINGS_KEY_LAYER_EXPRESSIONS)
        if layerExpressions is not None:
        ... do something usefull. eg. send message to browser ... 



```

# JavaScript
```javascript
$(function() {
    function AutostartPrintViewModel(parameters) {

        var PLUGIN_ID = "AutostartPrint"; // from setup.py plugin_identifier

        var self = this;

        // assign the injected parameters, e.g.:
        self.loginStateViewModel = parameters[0];
        self.settingsViewModel = parameters[1];

        // TODO: Implement your plugin's view model here.
    }

    /* view model class, parameters for constructor, container to bind to
     * Please see http://docs.octoprint.org/en/master/plugins/viewmodels.html#registering-custom-viewmodels for more details
     * and a full list of the available options.
     */
    OCTOPRINT_VIEWMODELS.push({
        construct: AutostartPrintViewModel,
        // ViewModels your plugin depends on, e.g. loginStateViewModel, settingsViewModel, ...
        dependencies: [
                        "loginStateViewModel",
                        "settingsViewModel"
                      ],
        // Elements to bind to, e.g. #settings_plugin_AutostartPrint, #tab_plugin_AutostartPrint, ...
        elements: [
            document.getElementById("autostartPrint_plugin_navbar"),
            document.getElementById("autostartPrint_plugin_settings")
        ]
    });
});
```

# JINJA2 Template

Filename *AutostartPrint_settings.jinja2*
```
<form id="autostartPrint_plugin_settings" class="form-horizontal" >
...
</form>
```
