# Figma Font-helper for Linux

This is a continuation of a project started by https://github.com/tryvin/.
This project is started because the original project was not being maintained.

I will try my best to respond and help people who want to contribute to this
repo by accepting pull requests etc.

# How

This project was a reverse engineer from the local font helper from Figma for
App, it uses fc-list, and fc-cache for the fonts lists, and python for
the webserver.

# How to use it

- Install Python3 and Pip3 (Pip for Python3)
- Run `pip install -r requirements.txt`
- Run `python server.py`
- Navigate to figma.com and try to use the local fonts.

You should see figma connecting in the log printed in the terminal when your
refresh figma.

## Create a deamon to always start the server on startup

Create a new systemd file:

`sudo nano /lib/systemd/system/figma-font-server.service`

```
# /lib/systemd/system/figma-font-server.service
[Unit]
Description=Figma Font server
After=network.target

[Service]
Type=simple
ExecStart=/home/my-username/.pyenv/shims/python /home/my-username/Projects/figma-linux-font-helper/server.py
User=<my-username>
Group=<my-username/group>
TimeoutSec=240
Restart=always
RestartSec=60


[Install]
WantedBy=network-online.target

```

In this file update the `ExecStart` line to point to your python installation
and your figma-linux-font-helper `server.py` file. Also update the `User` and
`Group` variables. These should normally be your username.

Then run:

`sudo systemctl enable figma-font-server.service`

Start the server for the first time:

`sudo systemctl start figma-font-server.service`

Check the status:

`sudo systemctl status figma-font-server.service`

And check the log output and refresh Figma to see that it is able to connect:

`sudo journalctl -f -u figma-font-server`

# Supported font formats:

.otf, .ttf, .ttc

To add more formats add them to line 61 in `helpers.py`
