a fork of https://gist.github.com/Chandler/fb7a070f52883849de35

_Note: The API calls were updated for the new `conversation` API; the OAuth Token instructions have been updated._

My fork is solely to document how to use the code with python3. If you are
unfamiliar with python3 but own a mac laptop.

The script finds all channels, private channels and direct messages
that your user participates in. it downloads the complete history for
those converations and writes each conversation out to seperate json files.

This user centric history gathering is nice because the official slack data exporter
only exports public channels.

PS, this only works if your slack team has a paid account which allows for unlimited history.

PPS, this use of the API is blessed by Slack.
https://get.slack.help/hc/en-us/articles/204897248

> If you want to export the contents of your own private groups and direct messages please see our API documentation.

## Obtaining the OAuth token

**IMPORTANT:** This script is written with the `Slacker` library which is deprecated. It uses the old token authentication mechanism. It must [be patched (just a few lines) to be able to work with new tokens](https://github.com/ismith/slacker/commit/8ff928c59ec77b047b7270d96936e3df7e2bc4fb) before use; it's best to do that in the local virtual environment generated by `pipenv` (see below).

1. Create a new app for the workspace (and select "From scratch"): https://api.slack.com/apps?new_app=1
2. Go to "Add features and functionality" and select "Permissions"
3. Add the following **user** permissions:

```
users:read
channels:history
channels:read
groups:history
groups:read
im:history
im:read
mpim:history
mpim:read
```

4. Near the top of the page click "[Re?]install App to Workspace"
5. Click the "OAuth & Permissions" tab, and scroll down to "OAuth Tokens for Your Workspace"
6. Copy the "User OAuth Token" (it will start with `xoxp-`)

#### A simple usage example

```shell
cd slack-history-export/
brew install python3
pip3 install pipenv
pipenv --python=/usr/local/bin/python3 shell
pipenv install
pipenv run python slack_history.py --token='123token'
```

#### other usage examples

```shell
pipenv run python slack_history.py --token='123token'
pipenv run python slack_history.py --token='123token' --dryRun=True
pipenv run python slack_history.py --token='123token' --skipDirectMessages
pipenv run python slack_history.py --token='123token' --skipDirectMessages --skipPrivateChannels
```

see [./slack_history.py](slack_history.py)
