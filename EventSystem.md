# Server-Side events:
https://docs.octoprint.org/en/master/events/

```python
def on_event(self, event, payload):
  if Events.PRINT_STARTED == event:
    self.alreadyCanceled = False
    self._createPrintJobModel(payload)
    ...
```

# Client-Side events (ViewModelCallbacks):
https://docs.octoprint.org/en/master/plugins/viewmodels.html#callbacks
