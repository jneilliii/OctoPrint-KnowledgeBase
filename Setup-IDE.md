# Install Octoprint

http://docs.octoprint.org/en/master/plugins/gettingstarted.html
https://github.com/foosel/OctoPrint/wiki/Setup-on-Windows

Make sure you have the right version (current 2.7.9)


    $ cd ~/devel
    $ git clone https://github.com/foosel/OctoPrint
    [...]
    $ cd OctoPrint
    $ virtualenv venv
    [...]
    $ source venv/bin/activate
    (venv) $ pip install -e .[develop,plugins]
    [...]
    (venv) $ octoprint --help
    Usage: octoprint [OPTIONS] COMMAND [ARGS]...

## exit venv

    $ deactivate

## Update to latest version
    $ pip install pip --upgrade
    $pip install https://get.octoprint.org/latest

# Start Octoprint from terminal
    
    $ octoprint serve

    Open Browser: http://localhost:5000/


# Setup PyCharm
Add Run Configuration

    Select "Python" and press +

    Name:				OctoPrint Server
    Script path:		D:\0_Projekte_private\3D_Printer\OctoPrint\OctoPlugs\OctoPrint\run
    Parameters:			serve --debug
    Python interprere:	Python 2.7 (venv)	
    Working directory:	D:\0_Projekte_private\3D_Printer\OctoPrint\OctoPlugs\OctoPrint

    External tool: Update dependencies
        Programm:		D:\0_Projekte_private\3D_Printer\OctoPrint\OctoPlugs\OctoPrint\venv\Scripts\pip.exe
        Arguments:		install -e .[develop,plugins]
        Working dir:	$ProjectFileDir$

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