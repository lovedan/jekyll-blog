---
title: SEND MAIL() USING PHP ON MAC OS X
categories:
- 苹果相关
- Mac技巧
date: 2017-09-01 10:47:40
tags: [Mac技巧,postfix,php,mail]
thumbnail: https://lh3.googleusercontent.com/-VaSX6FxYHOQ/Wai3loGBtnI/AAAAAAAADbQ/oyqO2I5bwPkzZpMvCsDU1F7I3AQ86_8uQCHMYCw/s0/2017-09-01_10-27-41.png
---
<!--excerpt-->

Thanks to Benjamin Rojas, Andy Stratton, and a tip from Jasper, I was able to successfully send email from my home-brewed MAMP environment. Here’s the summary.

1. Add the following to your /etc/postfix/sasl_passwd file:
```
smtp.gmail.com:587 username@gmail.com:password
```

(Of course, you don’t have to use GMail or port 587, but you get the idea.)

2. Configure postfix:

```
sudo postmap /etc/postfix/sasl_passwd
```

3. Backup and edit your postfix configuration:
```
sudo cp /etc/postfix/main.cf /etc/postfix/main.cf.orig
sudo vim /etc/postfix/main.cf
```

If you use TLS, then you will need to add the TLS settings but the other settings should already be there as a result of running the postmap command. You should have these options set in /etc/postfix/main.cf:

```
mydomain_fallback = localhost
mail_owner = _postfix
setgid_group = _postdrop
relayhost=smtp.gmail.com:587
smtp_sasl_auth_enable=yes
smtp_sasl_password_maps=hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options=
smtp_use_tls=yes
smtp_tls_security_level=encrypt
tls_random_source=dev:/dev/urandom
```

4. Start postfix:
```
sudo postfix start
```

If there are errors, you may need to edit your /etc/postfix/main.cf and restart postfix:
```
sudo postfix reload
```

5. Send a test message:
```
date | mail -s test youremailaddress@yourdomain.com
```

6. Make postfix start automatically on boot by opening your /System/Library/LaunchDaemons/org.postfix.master.plist file and adding:
```
<key>RunAtLoad</key>
<true/>
```

Add this at the bottom just before the closing </dict> tag.

7. Edit your /etc/php.ini file and configure the sendmail_path option:
```
sendmail_path = "sendmail -t -i"

```

You should now be able to send email using PHP’s mail() function. If you continue to have issues, watch the contents of your postfix mail log:
```
tail -f /var/log/mail.log
```
