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