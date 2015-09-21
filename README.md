### ssh email alerts using gmail SMTP servers

##### Ubuntu 14.04 / Linux Mint 17

##### Step 1:
##### Install the following packages

```bash
  $ sudo apt-get install bsd-mailx ssmtp

```

##### Step 2:
##### Modify the two following configuration files accordingly

**$ vi /etc/ssmtp/ssmtp.conf**

```
# /etc/ssmtp/ssmtp.conf
#
root=youralerts@gmail.com
mailhub=smtp.gmail.com:587
Hostname=youralerts@gmail.com
UseSTARTTLS=YES
AuthUser=youralerts@gmail.com
AuthPass=pass
FromLineOverride=yes
#UseTLS=YES
#Debug=YES
```

**$ vi /etc/ssmtp/revaliases**

```
 root:hostname:smtp.gmail.com
```

##### Step 3:
##### Add the following to the bottom of your /etc/bash.bashrc file

**$ vi /etc/bash.bashrc**
```
## ==== SSH Email Alerts ==== ##
if [ -n "$SSH_CLIENT" ]; then
  TEXT="$(date): ssh login to ${USER}@$(hostname -f)"
  TEXT="$TEXT from $(echo $SSH_CLIENT|awk '{print $1}')"
  echo $TEXT|mail -s "ssh login"  receiving@gmail.com
fi
```

##### Step 4:
##### Send a test email to your receiving email address

**method 1:**
```
  $ echo 'test email' |mail -s "Subject-01" receiving@gmail.com

```

**method 2:**
```
  $ vi msg.txt

      To: receiving@gmail.com
      From: youralerts@gmail.com
      Subject: test-subject

      This is a test email.
      Did this send?

  $ ssmtp receiving@gmail.com < msg.txt
```
