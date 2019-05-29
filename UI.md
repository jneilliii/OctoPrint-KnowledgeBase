- [h1](#h1)
  - [h3](#h3)

# Octoprint UI-Sections
The UI of Octoprint is seperated in the following areas. Each area could be adapted withj a Jina2-Template.

Just us the following namingconventions (path: ```/templates/'yourpluginId'_'area'.jinja2```)

Areas
* <plugin_identifier>_tab.jinja2			-> Tab
* <plugin_identifier>_navbar.jinja2		    -> Header
* <plugin_identifier>_sidebar.jinja2		-> Left sidebar
* <plugin_identifier>_settings.jinja2		-> Settings

# How does the UI-Communication work
For Server-Side rendering http://jinja.octoprint.org/ is used.

E.g. if you want to include HTML-Snippets you can use this:

```
{% include "snippets/settings/server/serverOnlineCheckHost.jinja2" %}
```
or for I18N-Support you can use this:
```
<p>{% trans %}Hello {{ user }}!{% endtrans %}</p>
```
Octoprint use https://knockoutjs.com/ for UI-Widget to JavaScript-Model Binding (2way binding)

![OverviewExample](images/knockout-example.png)
Source: https://knockoutjs.com/

## Knockout in Octoprint
1. Create JavaScript file and add it to the getAsset()-Section in ```__init__.py```

    Minimal JavaScript-File: PluginId.js

    ```javascript 
    $(function() {
    function PauseAtViewModel(parameters) {
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
        construct: PauseAtViewModel,
        // ViewModels your plugin depends on, e.g. loginStateViewModel, settingsViewModel, ...
        dependencies: [  "loginStateViewModel", "settingsViewModel"  ],
        // Elements to bind to, e.g. #settings_plugin_PauseAt, #tab_plugin_PauseAt, ...
        elements: [
            document.getElementById("pauseat_plugin_settings"),
            document.getElementById("pauseat_plugin_sidebar")
        ]
    });
});
    ```

2. Simple Textfield-Binding
   In your template:
   ```html
    ...
    <input type="text" name="myTextField" data-bind="value: myTextFieldValue">
    ...
   ```
   In your PluginId.js
   ```javascript
    ...
    self.myTextFieldValue = ko.observable();
    ...
   ```

3. Available ViewModels
    In the abouve example we saw some injected viewmodels, e.g. loginStateViewModel, here you can find an overview of all viewmodels:
    http://docs.octoprint.org/en/master/plugins/viewmodels.html
    
    Also on this page there are all "Event-Hooks" listed. E.g.:
   ```javascript
    ...
    // Data send from backend to frontend
    self.onDataUpdaterPluginMessage = function(plugin, data) {
    if (plugin != PLUGIN_ID) {
        return;
    }
    ...
   ```







# Hijack Print-Button

## h3

