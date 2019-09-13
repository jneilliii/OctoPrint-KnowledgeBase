Example sends Message-Data to the client, so that the Client could show a PopUp-Message Dialog.

```python
self._plugin_manager.send_plugin_message(self._identifier,
                                         dict(message_type=type,
                                              message_title=title,
                                              message_text=popUpMessage))
```

```javascript

self.onDataUpdaterPluginMessage = function(plugin, data) {
   if (plugin != PLUGIN_ID) {
     return;
   }
    
   if (data.message_text){
            new PNotify({
                title: data.message_title,
                text: data.message_text,
                type: data.message_type,  // 'notice', 'info', 'success', or 'error'.
                hide: false
                });
    }

}


```