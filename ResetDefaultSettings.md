TODO Screenshot

# Steps needed
1) Copy `ResetSettingsUtils.js` from one of my Repositories to `static/js/`
2) Include `ResetSettingsUtils.js` to `def get_assets(self)`
3) Add "reset-action" as API-Call
```phyton
    # to allow the frontend to trigger an update
    def on_api_get(self, request):
        if len(request.values) != 0:
            action = request.values["action"]
            
            # deceide if you want the reset function in you settings dialog 
            if "isResetSettingsEnabled" == action:
                return flask.jsonify(enabled="true")

            if "resetSettings" == action:
                self._settings.set([], self.get_settings_defaults())
                self._settings.save()

                return flask.jsonify(self.get_settings_defaults())
```
4) Assign default values after reset to the ViewModel in your JavaScript
```javascript
        var PLUGIN_ID = "AutostartPrint"; // from setup.py plugin_identifier       
        // enable support of resetSettings
        new ResetSettingsUtil().assignResetSettingsFeature(PLUGIN_ID, function(data){
                                // assign new settings-values // TODO find a more generic way
                                self.settingsViewModel.settings.plugins.DisplayLayerProgress.showOnNavBar(data.showOnNavBar);
                                ...
                                     
});

```
