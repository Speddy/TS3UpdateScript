# TS3UpdateScript

**TS3UpdateScript** is the most used bash-script by companies to automate and optimize the TeamSpeak 3 server update process. This is the fastest script to run this task.

[Show TS3UpdateScript Best Practices](docs/TS3US_BEST_PRACTICES.md)

The script checks if a newer version of TeamSpeak 3 server is available. If yes, you can choose if the script updates your server. You also set parameters like "delete old logs files" and so on. 

If you don't want to update your TeamSpeak 3 server manually, you can use this script. This script follows all of the steps to properly update your TeamSpeak 3 server.

## Various references

![Example Output](/img/TS3UpdateScript_Example_Summary.png)

[Open CHANGELOG](docs/CHANGELOG.md)

[Open install and usage guide](docs/INSTALL_USAGE_GUIDE.md)

[Open upgrade guide](docs/UPDATE_GUIDE.md)

## Developers

  * Sebastian Kraetzig ([@Sebi94nbg](https://github.com/Sebi94nbg))

## Contributors

[Open list of contributors](graphs/contributors)

## Donations

**TS3UpdateScript** is free software and is made available free of charge. Your donation, which is purely optional, supports me at improving the software as well as reducing my costs of this project. If you like the software, please consider a donation. Thank you very much!

[Donate with PayPal](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=7ZRXLSC2UBVWE)

## Main features

- Auto detection of
  - TeamSpeak server instances installation directories
  - Installed and latest (stable or beta) TeamSpeak server version as well as architecture and binary
    - You may will test the latest version for a while, so you can set 'your' latest version (View parameter '--set-version' with './TS3UpdateScript.sh --help')
  - File permissions of current installed TeamSpeak server
  - Used database type
  - TSDNS usage
  - ServerQuery IP and port
  - Latest version of the TS3UpdateScript self
- Backup as rollback, if the newer TeamSpeak server version is not compatible or not working
- Deletes old logs, if you want to (parameter '--delete-old-logs')
- Informs online clients via poke message, if you want to (parameter '--inform-online-clients')
- Test feature for informing online clients via poke message (parameter '--test-inform-online-clients')
- Keeps rollback backup, if you want to (parameter '--keep-backups')
- Individual waiting time between pokes and update process (parameter '--waiting-time-in-minutes')
- Backup and restore of temporary virtual server passwords
- Cronjob (CRON) for (weekly) fully automated update process (it's manually adjustable)
- Multiple language support (parameter '--locale')
- Easy use of debug function (parameter '--debug')

## Special features

- Pre-Validation of collected data to prevent possible update issues
- Best Practices Analyzer
- Debug feature, if you got some issues with the script
- Debug Report feature, if your update failed (Example [Debug Report](/docs/EXAMPLE_DEBUG_REPORT.md))

## Stay tuned!

- [facebook Fanpage](https://www.facebook.com/TS3Tools)
- [GitHub](/)

## Requirements

- Linux (should work on the most distributions; below a list of explicit tested distributions)
  - Debian
  - Ubuntu
  - CentOS
  - OpenSUSE
  - Fedora
  - IPFire
- One or more installed TeamSpeak 3 server instances on a Root server/VPS/virtual machine
  - Minimum required version: 3.1.x
- Software packages
  - Required
    - bash (GNU Bourne Again SHell)
    - which
    - ca-certificates (for SSL connections)
    - rsync (fast, versatile, remote (and local) file-copying tool)
    - wget (retrieves files from the web)
    - grep (GNU grep, egrep and fgrep)
    - sed (The GNU sed stream editor)
    - bzip2 (high-quality block-sorting file compressor - utilities)
    - lsb-release / redhat-lsb (Linux Standard Base version reporting utility)
    - gdb (GNU Debugger)
  - Optional
    - sqlite3 (Command line interface for SQLite 3) (Only required, if you've changed your IP bindings in the SQLite database)
    - any MTA like postfix or exim (for receiving cronjob mails)
    - expect (Automates interactive applications) (if you want to inform online clients about updates and get a higher detection rate of properties/settings)
    - telnet (The telnet client) (if you want to inform online clients about updates)
    - jshon (Read / Decode JSON format)
- root user access on your Linux system (below a list of some reasons, why the script needs root permissions)
  - Automated detection of one or more TeamSpeak 3 server instances
  - Detecting and changing/adjusting file/directory permissions after update process
  - Creating a backup
  - Installing a cronjob

## Supports

- Linux and FreeBSD binary
- systemd
- SQLite and MySQL as well as MariaDB database setups
- TSDNS
- Temporary virtual server passwords
- ExaGear environments (Further information: http://eltechs.com/)

## Hint: systemd

If you are using systemd to ensure, that your TeamSpeak server instance is running, you should add a condition to your systemd script to avoid automated restarts while the TS3UpdateScript is performing an update.

The TS3UpdateScript creates a temporary file called `.ts3updatescript.lock` in the root directory (where you also can find the `ts3server_startscript.sh` script) of the TeamSpeak server instance, when it starts updating the instance. As soon as it finished the update, it deletes this lock file again.

You can use this lock file to tell systemd to avoid automated restarts while the TS3UpdateScript is performing an update. Simply add a condition to systemd: `ConditionPathExists=!/home/teamspeak/.ts3updatescript.lock`

Here is a full example:

```
[Unit]
Description=TeamSpeak Server
After=network.target
ConditionPathExists=!/home/teamspeak/.ts3updatescript.lock

[Install]
WantedBy=multi-user.target
Alias=teamspeak-server.service

[Service]
User=teamspeak
Group=teamspeak
WorkingDirectory=/home/teamspeak/
ExecStart=/home/teamspeak/ts3server_startscript.sh start
ExecStop=/home/teamspeak/ts3server_startscript.sh stop
ExecReload=/home/teamspeak/ts3server_startscript.sh restart
PIDFile=/home/teamspeak/ts3server.pid
Restart=always
Type=forking
```

## Available Languages

Verified:
- en_US
- de_DE

Need Review:
- None

![Settings summary](/img/TS3UpdateScript_Settings_Summary.png)

### Help us

We cannot translate this script into each language. But you can help us achieve better language support by creating a new language file with your language! Please send your translation to our email address above, then we can add your translation to the next version. As thank you we will name you in this document, if you would like.

Just grab a copy of your language file, which you understand and translate the content between the two quotation marks. If you think it is ok, you can send us this file via email and we will add it to the TS3UpdateScript. You also can check it in the GitHub Project as new "Issue".

## Usage

All you need to do to get the list of available options/parameters is:

  user@tux:~$ sudo ./TS3UpdateScript --help

![Help menu](/img/TS3UpdateScript_Help_Page.png)

All you need to do to execute an update process is:

  user@tux:~$ sudo ./TS3UpdateScript --check

You also can use the tab auto-completion of this script, which will be installed after the first execution of the TS3UpdateScript and a relog of the Linux user:

  user@tux:~$ sudo ./TS3UpdateScript --check --[TAB][TAB]

  user@tux:~$ sudo ./TS3UpdateScript --check --dele[TAB]

## Parameters / Options

**Multi usable** parameters can be combined, while **single usable** can just be used without any other parameter(s). The order of any parameter is irrelevant - you can use them as you want to.

### Multi usable

Parameter | Description
:------------- | :-------------
--check | Checks, if a newer version for your instance is available
--delete-old-logs | Deletes old TeamSpeak 3 server logs while update process
--inform-online-clients | Sends the configured poke message to each online client on each virtual server, that the server will be updated (if you enter 'Yes, update!')
--keep-backups | Keep created backups by the script
--waiting-time-in-minutes [MINUTES] | Optionally the script can wait X minutes before it starts the update process
--disable-temporary-password-backup | Disable feature to backup temporary passwords
--set-release | Do not use the official latest release. Use instead the set release from the given file configs/config.all
--beta-release | With this parameter you are able to detect and update your TeamSpeak server to the latest beta release
--path | Specify the absolute path to the TS3 server instance, which should be updated instead of searching for all
--locale [LANGUAGE-CODE] | Change language of this script to some of the available under languages/verified/ directory. Default is en_US.
--debug [DEBUG_FILENAME] | Enables debugging and writes output to file
--install-cronjob | Installs weekly cronjob for monday at 3 AM (= 03:00 O'clock) with your given options

#### Examples

  user@tux:~$ sudo ./TS3UpdateScript --check

  user@tux:~$ sudo ./TS3UpdateScript --check --delete-old-logs

  user@tux:~$ sudo ./TS3UpdateScript --check --keep-backups

  user@tux:~$ sudo ./TS3UpdateScript --check --delete-old-logs --inform-online-clients 

  user@tux:~$ sudo ./TS3UpdateScript --check --delete-old-logs --waiting-time-in-minutes 30

  user@tux:~$ sudo ./TS3UpdateScript --check --delete-old-logs --waiting-time-in-minutes 30 --inform-online-clients --keep-backups

### Single usable

Parameter | Description
:------------- | :-------------
-h OR --help | Displays this help page
-v OR --version | Displays version of this script
--display-settings | Displays settings from config files in configs/
--test-inform-online-clients | Execute test poke with current settings
--update-script | Updates the TS3UpdateScript to the latest version
--deinstall-cronjob | Deinstalls weekly cronjob

#### Examples

  user@tux:~$ sudo ./TS3UpdateScript -h

  user@tux:~$ sudo ./TS3UpdateScript --help

  user@tux:~$ sudo ./TS3UpdateScript --display-settings

  user@tux:~$ sudo ./TS3UpdateScript --test-inform-online-clients

  user@tux:~$ sudo ./TS3UpdateScript --update-script

## Work flow of the script

First it will check if a newer version of the script is available; afterwards it will check your installed version of each instance against the latest available version from teamspeak.de. It also will detect if you are using TSDNS, SQLite or MySQL database, etc. If a newer version is available, the script will do following steps:

1. Download latest named TeamSpeak 3 server files from 4players.de
2. Inform online clients on all virtual servers (if you want to; parameter '--inform-online-clients' needed)
3. Stop running TSDNS (if used)
4. Backup temporary virtual server passwords (if any is available)
5. Stop running server instance gracefully
6. Delete old logs (if you want to; parameter '--delete-old-logs' needed)
7. Create backup of currently running TeamSpeak 3 server in '/tmp/TS3Tools/TS3UpdateScript/'
8. Unzip downloaded TeamSpeak 3 server files
9. Import licensekey (if available), database and Query IP Black and Whitelist from backup
10. Import TSDNS settings file (if used)
11. Delete ts3server.pid, if exists for correct start
12. Delete downloaded TeamSpeak 3 server files
13. Set ownership and group of files like before update
14. Start TSDNS (if was used)
15. Add commandline parameter to new ts3server_startscript.sh if INI-file is used
16. Start updated TS3 server
17. Restore temporary virtual server passwords (if any was backuped)
18. Script waits 15 seconds and checks if the server is running
19. If the server is not running, the rollback will be executed
20. Clean up system from created and downloaded files from the update process

Those steps will execute for each installed instance of Teamspeak 3, which will be checked.

The files/ directories will not be touched by the script - also not backuped!

## Used Resources by the script

Protocol | Host/IP  | Used for | How often?
:------------- | :------------- | :------------- | :-------------
https | www.teamspeak.com | For detection of latest stable server release version | Each execution of the TS3UpdateScript
https | files.teamspeak-services.com | Download server for TeamSpeak 3 server files | Each execution of the TS3UpdateScript
https | raw.githubusercontent.com | Server for checking latest TS3UpdateScript version | Each execution of the TS3UpdateScript
https | github.com | Download server for TS3UpdateScript files | Only if you update the TS3UpdateScript

## Directory Structure - Where can I find which file?

Path  | Description
:------------- | :-------------
.  | README.md, TS3UpdateScript.sh, .updateScript.sh, .ts3updatescript
./docs/  | Install and upgrade guide as well as changelog
./configs/  | Configuration files
./languages/  | Language files

## Why does this have a high version number?

Because of the TeamSpeak 3 Server updates, customer wishes and suggestions to the script, however, constantly extended or optimized, which is the reason why it will never have a "final" version level.

The script is ready and fully functional.
