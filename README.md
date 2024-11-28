## DayZ Server Manager for Linux
Custom script for managing your DayZ Server on Linux <br/>
Currently being used without issue on `#1 [UK|PCGAMERS]`

### WARNING
Use this [script](https://github.com/haywardgg/DayZ_Server_Manager/blob/main/dayzserver.sh) at your own risk. I've modified to suit my specific needs. <br/>
You are free to copy or fork the script and edit to your liking.<br/>
All I ask is that you don't remove the credits at the top of the main script. Thank you.

### Main Features
- Takes care of downloading SteamCMD and DayZ Server.
- Updates the DayZ Server and Mods at each startup/restart.
- Backs up your Profile and Storage folders at each startup/restart.
- Auto Start, Stop and Restarts are done with the help of Crontab.
- Simple to set up. 

## First RUN
1. Download [dayzserver.sh](https://raw.githubusercontent.com/haywardgg/DayZ_Server_Manager/5536718fb3361cf4f3baad9293f61918636e16c7/dayzserver.sh) to the root of your DayZ server home drive.
2. Then: `chmod +x dayzserver.sh`
3. Run the script: `./dayzserver.sh`
4. Wait until it has finished, then follow the instructions.
5. Edit the `.config.ini` file with your preferences.
6. Add your Mod ID's to the `.workshop.cfg` file.
   - Don't worry about adding the mod name. The script will do that later.
   - One ModID per line only.
   - Leave one space between the ID and the name `123456 ModName`
7. Edit the `serverfiles/battleye/beserver_x64_active_*.cfg`
   - Change the RCon password and port number, or leave as default.
   - Make sure the RCon and the Game Server ports aren't the same.
8. In your `serverfiles/serverDZ.cfg` file change your hostname and other settings.
   - Make sure there is a **steamQueryPort** setting. i.e `steamQueryPort = 27016;`
9. Run the script again and your server will be online within minutes.

## .workshop.cfg 
**Editing the `.workshop.cfg` file is real easy.**<br/>
- Add one workshop ID per line.<br/>
- The script will automatically check and append mod names each time you run: `./dayzserver.sh workshop`<br/>
- If a mod has been updated by the developer an optional notification can be sent to your Discord channel. <br/>
   - Just add your Discord URL in `.config.ini`
- You still need to manually add the `@modname(s)` to the `workshop=""` setting in `.config.ini`
   - This is because you might need to put them in a specific order, or remove one for testing, but not have to redownload it again.
- **.workshop.cfg** tells the script which MODS need to be downloaded. It does NOT add them to `workshop=""`. You do that yourself!

## Auto Restarts and Updates
The following cron jobs can be added to your DayZ Server users contab:<br/>
```
@reboot /home/dayz/dayzserver.sh start > /dev/null 2>&1
*/1 * * * * /home/dayz/dayzserver.sh monitor > /dev/null 2>&1
# */30 * * * * /home/dayz/dayzserver backup > /dev/null 2>&1
```
- Line 1 simply starts the server when the Linux machine is rebooted or turned on.
- Line 2 checks to see if the server is crashed or has been shutdown remotely, then restarts it.
- Line 3 (OPTIONAL) makes regular backups of your **storage** and **profile** folders.
   - By default the script backs up your folders during server startup/restart. 

You can use a service like CFTools to run server restarts (they have a free tier).<br/>
***CFTools "restart server schedular" shuts down the server and*** `/home/dayz/dayzserver.sh monitor` ***will restart the server***.<br/><br>

**MPORTANT NOTE**<br/> `monitor` will only restart the server if a remote shutdown (RCon) command was used. `dayzserver.sh shutdown` done from the Linux command line will shutdown the server and prevent `monitor` from restarting it.
