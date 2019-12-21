# Install OctoPrint

http://docs.octoprint.org/en/master/plugins/gettingstarted.html
https://github.com/foosel/OctoPrint/wiki/Setup-on-Windows

Make sure you have the right python version (current 2.7.9)

```
    $ cd ~/devel
    $ git clone https://github.com/foosel/OctoPrint
    [...]
    $ cd OctoPrint
    $ virtualenv venv
    [...]
    $ source venv/bin/activate
    (venv) $ pip install -e .[develop,plugins]
if not working, try:
    (venv) $ pip install -e .
    [...]
    (venv) $ octoprint --help
    Usage: octoprint [OPTIONS] COMMAND [ARGS]...

start octoprint:
    (venv) $ octoprint serve

```
## exit venv

    $ deactivate

## Update to latest version
    $ pip install pip --upgrade
    $pip install https://get.octoprint.org/latest

# Start Octoprint from terminal
```    
    $ octoprint serve
or
    $ OctoPrint serve

    Open Browser: http://localhost:5000/
```

# Setup PyCharm
Add Run Configuration
```
Select "Python" and press +

Name:			OctoPrint Server
Script path:		/Users/o0632/0_Projekte/3DDruck/OctoPrint/octoprint_latest/run
Parameters:		serve --debug
Python interpreter:	Python 2.7 (venv)
for Python 3 you need to add an Environment-Variable: LC_ALL=de_DE.utf-8	
Working directory:	/Users/o0632/0_Projekte_privat/3DDruck/OctoPrint/octoprint_latest

External tool: Update dependencies
Programm:		/Users/o0632/0_Projekte/3DDruck/OctoPrint/octoprint_latest/venv/bin/pip
Arguments:		install -e .[develop,plugins]
Working dir:	$ProjectFileDir$
```

# Add Virtual Printer

https://docs.octoprint.org/en/master/development/virtual_printer.html

edit in `config.yml`
```
# Settings only relevant for development
devel:

  webassets:
    bundle: false
	
  # Settings for the virtual printer
  virtualPrinter:

    # Whether to enable the virtual printer and include it in the list of available serial connections.
    # Defaults to fa
```

# Disable Safe-Mode

edit in `config.yml`
```
server:
    ignoreIncompleteStartup: true 
    startOnceInSafeMode: false
    ...
```