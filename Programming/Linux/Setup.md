#linux #font  
### How to fix font issue  
- Install Microsoft fonts  
- Install Segoe UI  
- Set default font to Segoe UI  
- Go to System Settings  
- Go to Fonts  
- General (Segoe UI)  
- Add below to ~/.config/fontconfig  
```xml  
<match>  
<test name="family">  
<string>Helvetica</string>  
</test>  
<edit binding="strong" mode="assign" name="family">  
<string>Segoe UI</string>  
</edit>  
</match>  
<match>  
<test name="family">  
<string>Arial</string>  
</test>  
<edit binding="strong" mode="assign" name="family">  
<string>Segoe UI</string>  
</edit>  
</match>  
```

### Default windows boot
https://unix.stackexchange.com/questions/639010/set-default-boot-entry-to-windows-instead-of-fedora

### Vscode
- Fix menu bar (Settings -> Search for "Title bar style" -> set to custom)
### Obsidian
- Clone using this
```
https://<PERSONAL_ACCESS_TOKEN>@github.com/<USERNAME>/<REPO>.git
```

### Useful commands
- check dns:
```bash
$ dig @dns domain
```
- networking
```bash
$ resolvectl
```

```bash
$ systemctl status systemd-resolved.service
```
- clear fontcache
```bash
$ fc-cache -fv
```
### Things that I changed
- Add dns: /etc/systemd/resolved.conf.d/dns_servers.conf (https://wiki.archlinux.org/title/systemd-resolved)
- Modify fonts: ~/.local/share/fonts
- Firefox homepage: /usr/lib64/firefox/browser/defaults/preferences/firefox-redhat-default-prefs.js
- nvidia-drm.modeset=1
- firefox config & /etc/environment (https://github.com/elFarto/nvidia-vaapi-driver)
- Qudelix rule (/etc/udev/rules.d):
```
# 96Hz
KERNEL=="hidraw*", ATTRS{idVendor}=="0a12", ATTRS{idProduct}=="4003", MODE="0666", GROUP="prdox"
# 88.2Hz
KERNEL=="hidraw*", ATTRS{idVendor}=="0a12", ATTRS{idProduct}=="4004", MODE="0666", GROUP="prdox"
# 48Hz
KERNEL=="hidraw*", ATTRS{idVendor}=="0a12", ATTRS{idProduct}=="4005", MODE="0666", GROUP="prdox"
# 44.1Hz
KERNEL=="hidraw*", ATTRS{idVendor}=="0a12", ATTRS{idProduct}=="4006", MODE="0666", GROUP="prdox"
# 44.1Hz/48/88.2/96Khz
KERNEL=="hidraw*", ATTRS{idVendor}=="0a12", ATTRS{idProduct}=="4007", MODE="0666", GROUP="prdox"
# 48Hz with Mic
KERNEL=="hidraw*", ATTRS{idVendor}=="0a12", ATTRS{idProduct}=="4125", MODE="0666", GROUP="prdox"
# 44.1Hz with Mic
KERNEL=="hidraw*", ATTRS{idVendor}=="0a12", ATTRS{idProduct}=="4126", MODE="0666", GROUP="prdox"
```