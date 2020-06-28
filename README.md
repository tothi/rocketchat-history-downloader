# Export and Download Rocket.Chat Logs and History

The rocketchat-history-downloader downloads and stores raw JSON channel history for joined channels, private groups, and direct messages. It keeps track of what has already been downloaded and supports ongoing periodic retrieval of new history.

Stores one JSON history file per channel per day in a directory of your choice.

## Getting Started

### Prerequisites

* Python >= 3.5
  * [Pipenv](https://github.com/pypa/pipenv), ideally
  * [rocketchat_API](https://github.com/jadolg/rocketchat_API) Python API wrapper for Rocket.Chat 
* A user account on a [Rocket.Chat](https://rocket.chat/) server that exposes its REST API

### Installing

Install requirements from Pipfile

    $ pipenv install
    
Create a directory to store downloaded history files

    $ mkdir history-files

Create and update settings.cfg

    $ cp settings.cfg.EXAMPLE settings.cfg

Example settings: 
```
[files]
history_output_dir = ./history-files/
history_statefile = rocketchat-history-statefile.pkl

[rc-api]
user = username_goes_here
pass = pwd_goes_here
server = https://demo.rocket.chat
max_msg_count_per_day = 99999
pause_seconds = 1
```

You may use `user_id` and `auth_token` instead of `user` and `pass` as an alternative authentication.

## Usage

Download all not-previously-downloaded history for all joined channels, private groups, and direct messages.
>Note: this updates the state file to record what has been downloaded so that it won't be downloaded again upon subsequent executions.

```
pipenv run python export-history.py settings.cfg
```

Download history for a specific time period. Update state file with last-downloaded time for all channels etc.

```
pipenv run python export-history.py -s 2000-01-01 -e 2018-01-01 settings.cfg
```

Run a one-off download of history for a specific time period and do not store last-downloaded dates (or anything else) in state file

```
pipenv run python export-history.py -s 2000-01-01 -e 2018-01-01 -r settings.cfg
```

### Logging

By default, debug and info output from any execution is written to the console and logged to a file called export-history.log
