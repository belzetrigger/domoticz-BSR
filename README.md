# domoticz-BSR
[![PyPI pyversions](https://img.shields.io/badge/python-3.6%20|%203.7%20|%203.8-blue.svg)]() [![Plugin version](https://img.shields.io/badge/version-1.1.7.-red.svg)](https://github.com/belzetrigger/domoticz-BSR/branches/)

Domoticz plugin that gets data of public waste collection for Berlin from the website of the [Berliner Stadtreinigungsbetriebe](http://www.bsr.de/).

![settings](https://github.com/belzetrigger/domoticz-BSR/raw/master/resources/unit_all_warning.PNG)

## Summary
This is a virtual hardware plugin that adds information about collecting days  from [bsr.de](https://www.bsr.de/abfuhrkalender-20520.php)  to your [Domoticz](https://www.domoticz.com/) interface. 
Therefore it generate one new alert sensor. Showing the next dates for the waste collection. If the day is coming closer the alarm level will change and finally show red.

As this company only works in Berlin - this plugin does it too.

This plugin is open source.


## Installation and Setup
- a running Domoticz, tested with 4.10717 and 2020.1
- Python 3
- install needed python moduls:
  - beautifullsoup4 aka bs4
  - you can use `sudo pip3 install -r requirements.txt` 
- clone project
    - go to `domoticz/plugins` directory 
    - clone the project
        ```bash
        cd domoticz/plugins
        git clone https://github.com/belzetrigger/domoticz-BSR.git
        ```
- or just download, unzip and copy to `domoticz/plugins` 
- no need on Raspbian for sys path adaption if using sudo for pip3
- some extra work for Windows or Synology, make sure downloaded modules are in path eg. site-packages python paths or change in plugin.py / fritzHelper.py path
  - example adaption:
    ```bash
    import sys
    sys.path
    sys.path.append('/usr/lib/python3/dist-packages')
    # for synology python3 from community
    # sys.path.append('/volume1/@appstore/python3/lib/python3.5/site-packages')
    # for synology sys.path.append('/volume1/@appstore/py3k/usr/local/lib/python3.5/site-packages')
    # for windows check if installed packages as admin or user...
    # sys.path.append('C:\\Program Files (x86)\\Python37-32\\Lib\\site-packages')
    ```
- restart Domoticz service
- Now go to **Setup**, **Hardware** in your Domoticz interface. There add
**BSR - Berlin Waste Collection**.
### Settings
   - best is to go  [bsr.de](https://www.bsr.de/abfuhrkalender-20520.php)
   - try to find your address
   - remember exactly the street name found for your address. Eg 'Kochstr.' instead of 'Kochstrasse'
   - if recycling is not collected at your house number, try the neighbors.  eg. using 10 instead of 10f

![settings](https://github.com/belzetrigger/domoticz-BSR/raw/master/resources/settings.PNG)


    - Street:
    - Zip code:
    - Number:
    - Update in hours
    - What kind of collection to show
        - 'waste' aka Restmüll or Hausmüll
        - 'recycling' aka Wertstoffe or gelber Sack
        - 'bio' biodegradable waste
        - 'xmas' shows collection dates for xmas trees
    -  Debug: 
       -  if True, the log will be hold a lot more output.
       -  false is the normal mode
       -  true detail -> also shows response on log
       -  True fast full detail -> handles poll time as minutes, for faster debugging
  
## Bugs and ToDos
- using Locale for dates,months, days ... works good on windows, breaks on linux
- add feature to create summary device or/and device per type
- own alarm pictures matching the waste type
- Street names often have just "...str." not "....strasse" 
- similar street names within same zip code eg: 
    - Am Falkenberg 
    - Am Falkenberg am Wasserwerk
    - booth in 12524
- mehrere Abholungen an einem Tag

## State
Runs now for several years without any issue... Might get some updates for Domoticz 2020.1. 

## Developing
Based on https://github.com/ffes/domoticz-buienradar/ there are
 -  `fakeDomoticz.py` - used to run it outside of Domoticz
 -  `testBsr.py` it's the entry point for tests

## ChangeLog
1.0.0 init
1.1.0 added more debug options and also display xmas tree collections date
1.1.1 now xmas tree collection date is only shown in December and January
1.1.2: for device name: removed content in '(foo)' from waste type, to keep it short
1.1.3: if there is an error, ignore polling time and try with next heart beat again
1.1.4: update also if we have a day change between last update and heartbeat, so we will get correct device name with numbers of days
1.1.5: small fix to ignore dates older then today entries for waste disposal, eg. xmas tree always returned full list.
1.1.6: new debug parameter 'True fast full detail'. if it is turned on, handle update intervale as minutes not hours!
1.1.7: bugfix, forgot to clear data storage before reading them from webservice

