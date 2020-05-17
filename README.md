# FreeGamesonSteam
Searching SteamDB for Free Games and Activating them using  ArchiSteamFarm 

# Important!
You need to enable the IPC interface.

Put this in your ASF.json:
```
{
	"IPC": true
}
```

and

You need to enter your bot name in the script file!
```
...
# Config
bot_name = "PUT_YOU_BOT_NAME_HERE"
...
```

# Setup for Raspberry-Pi

1. You need ArchiSteamFarm running on ``` 127.0.0.1:1242 ```
2. Make the Directory and change in it: ``` mkdir /home/pi/steambot && cd /home/pi/steambot ```
3. Download the Script and Config: ``` wget https://raw.githubusercontent.com/Nickwasused/FreeGamesonSteam/master/steam.py && wget https://raw.githubusercontent.com/Nickwasused/FreeGamesonSteam/master/steamconfig.py```
4. Install Dependencies ```  sudo pip3 install beautifulsoup4 ```
5. Create the Service and timer file:
	- Path: ``` /etc/systemd/system/steam.service```
	- Content : 
	```
	[Unit]
	Description=Steam service
	After=network.target
	StartLimitIntervalSec=0

	[Service]
	Type=simple
	User=pi
	ExecStart=/usr/bin/python3 /home/pi/steambot/steam.py
	WorkingDirectory=/home/pi/steambot/

	[Install]
	WantedBy=multi-user.target
	```
				
	- Path: ``` /etc/systemd/system/steam.timer```
	- Content : 
	```
	[Unit]
	Description=Execute Steam

	[Timer]
	OnCalendar=*-*-* 0,6,12,18:00:00
	Unit=steam.service

	[Install]
	WantedBy=multi-user.target
	```
	
6. Enable and Start the Services:
	- ``` sudo systemctl enable steam.service ```
	- ``` sudo systemctl enable steam.timer ```
	- ``` sudo systemctl start steam.service ```
	- ``` sudo systemctl start steam.timer ```

# Notice

The Service assumes that the Script is located here: ``` /home/pi/steambot/steam.py ```
And the Service assumes that the Config is located here: ``` /home/pi/steambot/steamconfig.py ```
