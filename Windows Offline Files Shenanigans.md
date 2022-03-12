# Windows Offline Files

I have a samba share that I planned to access with my laptop on the go, via a VPN. At the same time, I would use the offline files feature to sync files that I needed at all times, especially when I was on school Wi-Fi (which blocks VPNs). However, after setting everything up, I noticed that I could only connect to my samba share very occasionally via my VPN, so I set out to investigate.

After an hour or two, I stumbled upon the event viewer, and one particular event caught my eye, Event ID 1004.

```
Log Name: Microsoft-Windows-OfflineFiles/Operational
Source: OfflineFiles
Logged: 12/3/2022 2:30:47 pm
Event ID: 1004
Task Category: None
Level: Information
Keywords: Online/offline transitions
Description:
Path \\server\Folder transitioned to slow link with latency = 50 and bandwidth = 407144
```

The path of the above event can be found here.

```
Event Viewer (Local) \ Applications and Services \ Microsoft \ Windows \ Offline Files \ Operational
```

This indicated that we were switching to offline mode when the latency hits 35ms. This has to be changed, and we can do this via the Group Policy Editor.

1. Open the Group Policy Editor
2. Go to the following path:  
   *Computer Configuration -> Policies -> Administrative Templates -> Network -> Offline Files -> Configure slow-link mode*
3. Change the setting to "Enabled"
4. Scroll to the bottom of the "Options" box & click on the "Show" button
5. Key in "*" into the first row of the "Value Name" column  
   *This allows us to apply this policy to all servers. If you would like to apply this to specific servers, you can key in something like "\\\servername\\\*"*
6. Key in "100" into the first row of the "Value" column
   *You can choose any latency you want. You can take some time to experiment with this setting.*
7. Click "OK" twice
8. Open up command prompt, and execute the following command
   ```
   gpupdate /Target:Computer
   ```
9. Restart your computer

You should test your network drives again, and see if they still disconnect. Change the latency setting where needed.

This was tested on my Windows 11 with the 21H2 update.

Enjoy your offline files!