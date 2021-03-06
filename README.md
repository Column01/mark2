# mark2

     mark2  server01  server02                                        user
    ┌────────────── server01 ──────────────┐┌─────────── stats ───────────┐
    │2015-06-04 13:55:34 | Server          ││cpu: 0.20%                   │
    │permissions file permissions.yml is   ││mem: 2.06%                   │
    │empty, ignoring it                    ││load: 0.10, 0.14, 0.11       │
    │2015-06-04 13:55:35 | Done (3.522s)!  ││players: 0 of 300            │
    │For help, type "help" or "?"          │└─────────────────────────────┘
    │2015-06-04 13:55:35 | Using epoll     │┌────────── players ──────────┐
    │channel type                          ││                             │
    │2015-06-04 13:55:35 | [NoCheatPlus]   ││                             │
    │Post-enable running...                ││                             │
    │2015-06-04 13:55:35 | [NoCheatPlus]   ││                             │
    │Post-enable finished.                 ││                             │
    │2015-06-04 13:56:16 # user attached   ││                             │
    │2015-06-04 13:56:24 # user detached   ││                             │
    │2015-06-04 14:00:37 # user attached   ││                             │
    └──────────────────────────────────────┘└─────────────────────────────┘
     >

mark2 is a minecraft server wrapper, written in python and twisted. It aims to be *the* definitive wrapper, providing a
rich feature-set and a powerful plugin interface. It has no requirement on craftbukkit.

See [INSTALL.md](INSTALL.md) for requirements and installation instructions

See [USAGE.md](USAGE.md) for details on how to use mark2

## Features

* Your server runs in the background
* Multiple users can attach at once, with their own local prompt and command buffer
* Built in monitoring using cpu, memory, players and connectivity
* Rich screen/tmux-like client with built-in monitoring, tab-complete, command history, etc

## Plugins

* Powerful scheduling plugin, with a cron-like syntax. You can hook onto events like `@serverstopped` to run a
  cartograph, or run `save` on an interval
* Automatically restart the server when it crashes, runs out of memory, or stops accepting connections
* Notifications via Prowl, Pushover, NotifyMyAndroid or email if something goes wrong.
* Relay in-game chat to IRC, and vice-versa
* MCBouncer ban support, even on a vanilla server.
* Read an RSS feed (such as a subreddit feed) and announce new entries in-game
* Back up your map and server log when the server stops
* Print a random message at an interval, e.g. '[SERVER] Lock your chests with /lock'
* Respond to user commands, e.g. '<Notch> !teamspeak' could `msg Notch Join our teamspeak server at xyz.com`

## Common Issues

### `ServerStarted` event does not run scripts properly.

This can happen due to the done message in the prompt from starting the server not being caught. Edit the `mark2.properties` in the server directory and add a line like the following that matches the "Done" message for when the server fully starts.

A typical done message regex looks like this: `mark2.service.process.done-pattern=\\[(.*?)\\]: Done \\(([0-9]*\.?[0-9]*)s\\)\\!.*`

### mark2 says server may have crashed or is not responding
The regex for the unknown command message is broken. In order to set it for that server, edit the `mark2.properties` in the server directory and add a line like the following that matches the unknown command message: 

`plugin.monitor.crash-unknown-cmd-message=\\[(.*?)\\]: Unknown command.*`

### Player list on the right hand side is empty
This happens when the join/quit regexes are not set properly for that server. Edit the `mark2.properties` in the server directory and add a line like the following that matches the join/quit messages in the console:

Typical join regex: `mark2.regex.join=\\[(.*)\\]: (?P<username>[A-Za-z0-9_]{1,16}).*\\[\/(?P<ip>[0-9.:%]*)\\] logged in with entity id .+`

Typical leave regex: `mark2.regex.quit=\\[(.*)\\]: (?P<username>[A-Za-z0-9_]{1,16}) lost connection: (?P<reason>.+)`

### Missing process information in the right side panel

This usually happens when the `psutils` module is installed incorrectly or for the wrong python version. You should double check you installed it properly using `pip install psutil` on ubunutu or install it manually in the python 2 environment

### mark2 has some sort of python error or exception upon starting

This can happen for a few reasons but the most likely reason is you are trying to run mark2 using python 3. In the terminal, run `python -V`, ensure that it is a version of Python 2 (Python 2.7 is best). If for some reason the above command says python 3, you need to change your system to point the `python` command to a python 2 install. Once you point the python command to the correct python version, you'll have to install all the requirements again. Once you do that, you shouldn't have python errors when running mark2.
