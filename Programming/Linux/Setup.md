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
- Clone using this (https://<PERSONAL_ACCESS_TOKEN>@github.com/<USERNAME>/<REPO>.git)