# DisplayChange
This is forked from MartinGC94/DisplayConfig with the original readme info after "DispalyConfig".
DisplayChange app provides a simple way to switch between different display configurations. 
I use this to switch between a local ultrawide "display 1" at 5120x1440 and "display 2", which is a 4K HDMI Virtual Display Adapter at 2560x1600.
I am using these two resolutions because it matches my setup of ultrawide monitor and surface pro for streaming, but you can make changes to that with simple edits to the DisplayChange.HTA file.
If there is any intrest, I'll make a veriable so you can set it to whatever you want, and add additional configuration options to choose from. 

# How To Install 
```
Open PowerShell as Administrator 
Run Command: Install-Module DisplayConfig
Close PowerShell
Download DisplayConfig.hta and save it to the desktop or whereever you want
Open DisplayConfig.hta with Microsoft (R) HTML Application Host (set this as the default app to open HTA files)
```

# Other Tips 
I created this solution for quickly fixing resolution problems when streaming games to my surface. Without this setup, the resolution on the end device would never look right. 
Install Sunshine on your host computer and Moonlight on your end device. 
Buy a 4K HDMI Virtual Display Adapter Dongle for $5 and plug it into your PC HDMI, set it as display 2 and your ultrawide as display 1.
Configure your bios to "wake from LAN". 
Download a "wake from LAN" app on your phone, or use the one built into Moonlight (the later I've had some issues with, so I use an app to wake the PC). 
Make sure Sunshine is set to launch on startup on the host.
On the end device, open Moonlight once the PC has booted up. You should see the windows login screen. If on Android, tripple tap (one tap, three fingers), to bring up virtual keyboard, otherwise plug in a keyboard. Enter your password and log into windows. In most cases you should be able to see the login screen since sunshine should be running before you login. If not, you can disable pw for login in windows, which is not secure. 
Open DisplayConfig.hta (with Microsoft (R) HTML Application Host). I have copied the file to my startup directory so it launches on boot.
Pick if you want the local or streaming setup. If you select "streaming' your local ultrawide will go black. Your end device should have 2560x1600 resolution (or whatever you changed it to). Once you are done streaming, hit "local" and the ultrawide should come back up. If you left the stream open you'll notice it looks wonky again. 
Moonlight will allow you to set the resolution and FPS for your stream. It is a good idea to set this to "Native" after you have your stream looking good on the end device. You will want your virtual display to match the streaming resolution for best preformance. It's also good to adjust the FPS to whatever the virtual display is running (or where your game FPS is locked), for best performance. Set the bitrate as high as your network will allow. I have the host PC hardwired into the network and 300 Mbp/s wifi connection with my end device with no issues.
There is a known issue with the steamdeck wifi chip- even if it looks like you have a good connection the chip itself is bad and will cause you issues. If you want high framerate on steamdeck streaming, get a USB to ETH dongle and hardwire the deck in. I ended up just switching to tablet + xbox controller. I have heard very good things about the current generation of AMD minicomputers- for around $300 you can get a top of the line streaming box that will connect to any HDMI display.  

# DisplayConfig
This PowerShell module allows you to manage various display settings like: Resolution, scaling, desktop positions and more.
It's built on top of the CCD (Connecting and Configuring Display) APIs. For more details, see: https://learn.microsoft.com/en-us/windows-hardware/drivers/display/ccd-apis

# Getting started
Install the module from the PowerShell gallery: `Install-Module DisplayConfig`  
Then check the available commands in the module: `Get-Command -Module DisplayConfig`  
There are 2 ways to use the commands in this module. First there's the simple way where you just call each individual command and the settings are immediately applied when each command finishes. For example:
```
Set-DisplayResolution -DisplayId 1 -Width 2560 -Height 1440
Set-DisplayRefreshRate -DisplayId 1 -RefreshRate 165
```
The other way is to generate a display configuration with all the desired settings, which can then be used to apply all the specified settings at once. For example:
```
Get-DisplayConfig |
    Set-DisplayResolution -DisplayId 1 -Width 2560 -Height 1440 |
    Set-DisplayRefreshRate -DisplayId 1 -RefreshRate 165 |
    Use-DisplayConfig
```
The benefit of the second approach is that it reduces the amount of time it takes to change all the settings and it reduces the amount of flickering that would normally occur whenever a display setting is changed.  
Another benefit is that a configuration can be backed up to a file and later restored:
```
Get-DisplayConfig | Export-Clixml $home\TVGamingProfile.xml
# Note that you need to import the module in the new session before importing the XML otherwise PowerShell will fail to convert the object correctly.
Import-Module DisplayConfig
Import-Clixml $home\TVGamingProfile.xml | Use-DisplayConfig -UpdateAdapterIds
```
Due to API limitations, not all commands support the display config approach. You can view the list of commands that do by running: `Get-Command -Module DisplayConfig -ParameterName DisplayConfig`.
