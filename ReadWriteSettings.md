# Validate settings after save

```
def on_settings_save(self, data):
        layerExpressions = data.get(SETTINGS_KEY_LAYER_EXPRESSIONS)
        if layerExpressions is not None:
        ... do something usefull. eg. send message to browser ... 
```