# DayZ Server Manager for Linux — Quick & Friendly Guide 🚀

> This project provides a small bash script (`dayzserver.sh`) to manage a DayZ server on Linux: install SteamCMD, download/update DayZ server files and Workshop mods, perform backups, and help with automatic restarts.

Note: This repository is community-maintained. Use at your own risk.

**If you like the project, keep the credits at the top of `dayzserver.sh`.** Thanks! 🙏

---

## 🎯 What it does (quick)
- Installs SteamCMD and the DayZ server files
- Downloads and updates Steam Workshop mods listed in `workshop.cfg`
- Creates backups of mission and profile data
- Provides start/stop/restart/monitor/console commands via `dayzserver.sh`

---

## 🧰 Prerequisites
- A Linux server with a normal user account (do NOT run as `root`)
- Basic tools: `tmux`, `curl`, `jq`, `wget`, and the 32-bit C runtime (`lib32gcc-s1` on Debian/Ubuntu)

Install example (Debian/Ubuntu):

```bash
sudo apt update
sudo apt install tmux curl jq wget lib32gcc-s1 -y
```

---

## 🚀 Quick Start (complete novice)
1. Download the script to your server home directory (example: `/home/dayz`):

```bash
cd ~
curl -O https://raw.githubusercontent.com/haywardgg/DayZ_Server_Manager/main/dayzserver.sh
chmod +x dayzserver.sh
```

2. Run the script interactively (it will create `config.ini` if missing):

```bash
./dayzserver.sh
```

3. Open `config.ini` and update the required settings (at minimum set `steamlogin`):

```ini
# Example
steamlogin=anonymous        # or "username password"
port=2301
config=serverDZ.cfg
```

4. Add Workshop mod IDs into `workshop.cfg` (one numeric ID per line). The script will try to lookup mod names automatically.

5. Start your server:

```bash
./dayzserver.sh start
```

---

## 📝 `workshop.cfg` (mods)
- Add one Workshop mod ID per line. Example:

```
123456789
987654321
```

- The script will download mods using SteamCMD, place them under `serverfiles/steamapps/workshop/content/221100`, and create `@modname` folders inside `serverfiles`.
- If you want mods enabled in the server launch line, add `@modname;@modname2` to `workshop` in `config.ini` (lowercase recommended).

---

## ⏰ Auto restarts & monitoring (cron)
Recommended crontab for the DayZ user (`crontab -e` as the dayz user):

```cron
@reboot /home/dayz/dayzserver.sh start > /dev/null 2>&1
*/1 * * * * /home/dayz/dayzserver.sh monitor > /dev/null 2>&1
# Optional backup every 30 minutes
# */30 * * * * /home/dayz/dayzserver.sh backup > /dev/null 2>&1
```

- `monitor` will restart the server if it crashed or was shut down via RCON/messages.xml. If you intentionally stop the server with `./dayzserver.sh stop`, `monitor` will not start it again (by design).

---

## 🔧 Configuration pointers
- Edit `config.ini` for launch parameters, ports, and Discord webhook URL for mod update notifications.
- Set RCon and game ports in `serverfiles/battleye/beserver_x64*.cfg` and `serverDZ.cfg` (do not use the same port for both).
- Confirm `steamQueryPort` is set in `serverfiles/serverDZ.cfg`, e.g. `steamQueryPort = 27016;`

---

## 🛠️ Common commands
- Install & validate server files:

```bash
./dayzserver.sh install     # install steamcmd and server files
./dayzserver.sh validate    # validate server files with SteamCMD
```

- Start/stop/restart/console:

```bash
./dayzserver.sh start
./dayzserver.sh stop
./dayzserver.sh restart
./dayzserver.sh console    # attach to tmux console (follow prompts)
```

---

## 🆘 Troubleshooting
- Script warns and exits if run as `root`. Run as a regular user.
- If SteamCMD fails, try running the SteamCMD command manually from `~/steamcmd` to inspect output.
- Check `~/.dayzlockfile` and `~/.dayzlockupdate` for status during updates/monitoring.

---

## ❤️ Need help or want improvements?
- I can help modernize the script further, add systemd service support, or produce an install guide with screenshots. Ask me to:

- convert cron-based monitoring to a `systemd` service
- add safer locking / logging
- produce a step-by-step beginner tutorial with screenshots

---

Made with ❤️ — enjoy running your DayZ server!
