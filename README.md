# Tutorial for Desktop enviroment in WSL2 (Arch distro)
The tutorial will tell you how to run desktop environment inside Windows Subsystem for Linux.   
And you don't need to build a developement environment with virtual machines any more.

## Prerequisites
Please note that you'll need to be running **Windows 10 Build 19041 or higher**.  
You will also need to have (WSL2)[https://docs.microsoft.com/en-us/windows/wsl/install-win10#update-to-wsl-2] installed.  
To install the Arch distro follow (these)[https://github.com/yuk7/ArchWSL] instructions.  
  
This tutorial should also work for other WSL distro's if u know linux a bit.  


## Set Arch to be backed by WSL 2
In PowerShell run:
```powershell
wsl --set-version Arch 2
```

If you don't use Arch, make sure to replace Arch with the actual name of your distro. (You can find these with the command: wsl -l).  
  
Additionally, run the command below to make WSL 2 your default architecture:
```powershell
wsl --set-default-version 2
```

## Install VcXsrv
Install the lastest version of [VcXsrv](https://sourceforge.net/projects/vcxsrv/).

## Upgrade Arch
```bash
sudo yay -Syu
sudo pacman -Syu
```

## Specify the display server
Add bellow code to `~/.bashrc`.

```bash
export DISPLAY=`grep -oP "(?<=nameserver ).+" /etc/resolv.conf`:0.0
export LIBGL_ALWAYS_INDIRECT=1
```

Then don't forget to run `source ~/.bashrc`.

## Couple quick changes
Edit `/usr/share/dbus-1/session.conf` with sudo permissions & replace following lines:

```xml
<listen>unix:tmpdir=/tmp</listen>
```
with
```xml
<listen>tcp:host=localhost, port=0</listen>
```
and replace
```xml
<auth>EXTERNAL</auth>
```
with
```xml
<auth>ANONYMOUS</auth>
<allow_anonymous/>
```

## Edit windows firewall
Firewall & network protection -> Allow an app through the firewall ->   
Change settings -> Allow another app -> Path: 'C:\Program Files\VcXsrv\vcxsrv.exe'

## Startup
Open XLaunch, choose "One large window" or "One large window without titlebar", set the "display number" to 0 and click "Next.  
Choose "Start no client" and click "Next".  
Check "Disable access control" and click "Next".  
Click "Finish" to launch display server.  
  
After this start xface4 in Arch: `startxfce4`  
  
Enjoy! :)  


For further debugging if you encounter problems see (this)[https://github.com/microsoft/WSL/issues/4106] thread.  
OG thread: https://github.com/microsoft/WSL/issues/637
