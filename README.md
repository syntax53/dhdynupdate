# DreamHost Dynamic DNS Record Update
This is a windows fork of (https://github.com/ttelford/dhdynupdate).

## Features Added

* Windows Support.
* Ability to lookup your external IP and send that to dreamhost.
* Save and restore previous addresses from file.

## Dependencies
The `daemon` library **not** required as it is with the linux version (it's not supported under windows).  You can setup a scheduled task in windows instead.

* [`python`](https://www.python.org/downloads/windows/)
    *  Python **3.4** is requred (as of this writing) because netifaces doesn't support 3.5.
* [`netifaces`](https://pypi.python.org/pypi/netifaces)
* [`requests`](http://docs.python-requests.org/en/latest/)
* [`lockfile`](https://pypi.python.org/pypi/lockfile)

# Installation
* Install Python (assumingly to C:\Python34)
* Open an elevated command prompt to `C:\Python34\Scripts`
* Execute `pip install netifaces`
* Execute `pip install requests`
* Execute `pip install lockfile`
* Create a folder where you want to put dhdynupdate (I chose `C:\Python34\_dhdynupdate`) and put the files in there.

# Configuration
* Get your interface's GUID...
    * Back in `C:\Python34` run python by executing `python`
    * Execute `import netifaces`
    * Execute `netifaces.interfaces()` to get a list of all of your interface GUIDs.
        * You can execute `netifaces.ifaddresses('{GUID}')` on each listed guid to find the one you want.  It will return the MAC and IP, among other properties.
        * You can also browse to the following registry key which may be easier: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkCards`. Verify you have the correct GUID (and the library is working) with `netifaces.ifaddresses('{GUID}')`.
* Open `dhdynupdate.conf` and put your GUID next to the AF_INET entry.
* Specify the location of the logfile (`C:\Python34\_dhdynupdate\log\dhdynupdate.log` for me).  Make sure the folder/file is writable.
* Specify the location of the previous address file (`C:\Python34\_dhdynupdate\log\prev_addr.txt` for me).  Make sure the folder/file is writable.
* See https://github.com/ttelford/dhdynupdate for getting your API key.
* Modify `[your.domain.com]` to anything you want.  e.g. `[myroot.domain.com]`.
* `api_key` = `<your DreamHost API key>`
* `local_hostname` = `yourhost.toupdate.com`
* Once you have the command working and verify it's working via the log, just setup a windows task to execute the command at a set interval.

# Command-line usage

	usage: dhdynupdate.py [-h] [-d] [--debug lvl] [-c config]
	
	optional arguments:
	  -h, --help            show this help message and exit
	  --debug lvl           Log Level, one of CRITICAL, ERROR, WARNING, INFO,
	                        DEBUG
	  -c config, --config config
	                        Configuration name

* Execute `C:\Python34\python.exe C:\Python34\_dhdynupdate\dhdynupdate.py -h` just to make sure it executes.
* Execute `C:\Python34\python.exe C:\Python34\_dhdynupdate\dhdynupdate.py -c your.config.section.name` to update.
* Normally very little information is written to the logfile.  Add ` --debug DEBUG` to the end of your command to see everything it's doing.
