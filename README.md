# DreamHost Dynamic DNS Record Update
This is a windows fork of (https://github.com/ttelford/dhdynupdate).

## Dependencies
The `daemon` library **not** required (it's not supported under windows for obvious reasons).  You can setup a scheduled task in windows instead.

* [`python`](https://www.python.org/downloads/windows/)
    *  Python **3.4** is requred (as of this writing) because 
* [`netifaces`](https://pypi.python.org/pypi/netifaces)
    * Available in Debian/Ubuntu as `python3-netifaces`
* [`requests`](http://docs.python-requests.org/en/latest/)
    * Available in Debian/Ubuntu as `python3-requests`

# Installation
* Install Python (assumingly to C:\Python34)
* Open an elevated command prompt to `C:\Python34\Scripts`
* Execute `pip install netifaces`
* Execute `pip install requests`
* Create a folder where you want to put dhdynupdate (I chose `C:\Python34\_dhdynupdate`). And put the files in there.

# Configuration
* Get your interface's GUID...
    * Back in `C:\Python34` run python by executing `python`
    * Execute `import netifaces`
    * Execute `netifaces.interfaces()` to get a list of all of your interface GUIDs.
        * You can execute `netifaces.ifaddresses('{GUID}')` on each listed guid to find the one you want.  It will return the MAC and IP, among other properties.
        * You can also browse to the following registry key which may be easier: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkCards`. Verify it's the correct GUID with `netifaces.ifaddresses('{GUID}')`.
* Open `dhdynupdate.conf` and put your GUID next to the AF_INET entry.
* Specify the location of the logfile (`C:\Python34\_dhdynupdate\log\dhdynupdate.log` for me).  Make sure the folder/file is writable.
* See https://github.com/ttelford/dhdynupdate for getting your API key.
* Modify `[your.domain.com]` to anything you want.  e.g. `[myroot.domain.com]`.
* `api_key` = `<your DreamHost API key>`
* `local_hostname` = `yourhost.toupdate.com`

# Command-line usage

	usage: dhdynupdate.py [-h] [-d] [--debug lvl] [-c config]
	
	optional arguments:
	  -h, --help            show this help message and exit
	  --debug lvl           Log Level, one of CRITICAL, ERROR, WARNING, INFO,
	                        DEBUG
	  -c config, --config config
	                        Configuration name
