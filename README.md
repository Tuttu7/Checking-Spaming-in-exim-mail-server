#### To check the number of emails present in the queue:

exim -bpc

#### To check the emails present in the queue with the mail id and sender ID:

exim -bp
exim -bp | less

#### To view the header of a particular email using mail ID :

exim -Mvh mail_id


#### To view the body of a particular email using mail ID:
 
exim -Mvb mail_id

#### To view a message's logs:

exim -Mvl mail_id

#### To trace path:

exim -d -bt user@domain.com

#### To get sorted list of email sender in exim queue:
 
exim -bpr | grep "<" | awk {'print $4'} |cut -d "<" -f 2 | cut -d ">" -f 1 | sort -n | uniq -c| sort -n


#### Following command that will show you the script which is using script to send the email. If it is from php then use

egrep -R "X-PHP-Script"  /var/spool/exim/input/*

#### The above command is valid only if the spamming is currently in progress. If the spamming has happened some hours before, use the following command.

grep "cwd=/home" /var/log/exim_mainlog | awk '{for(i=1;i<=10;i++){print $i}}' | sort | uniq -c | grep cwd | sort -n

#### To check the script that will originate spam mails:

grep "cwd=/home" /var/log/exim_mainlog|awk '{for(i=1;i<=10;i++){print $i}}'|sort| uniq -c|grep cwd|sort -n

#### If we need to find out exact spamming script. To do this, run following command:

 ps auxwwwe | grep user | grep --color=always "/home/user/public_html/templates/" | head
 
#### To delete the emails of a specific user:

grep -lr 'user@domain.com' /var/spool/exim/input/ | sed -e 's/^.*\/\([a-zA-Z0-9-]*\)-[DH]$/\1/g' | xargs exim -Mrm

exim -bp | grep "user_email-account" | awk '{print $3}' | xargs exim -Mrm

#### To delete Frozen emails from the email queue:

grep -R -l '*** Frozen' /var/spool/exim/msglog/*|cut -b26-|xargs exim -Mrm

exim -bp| grep frozen | awk '{print $3}'| xargs exim -Mrm

exiqgrep -z -i | xargs exim -Mrm

#### To delete Spam emails from the email queue:

grep -R -l [SPAM] /var/spool/exim/msglog/*|cut -b26-|xargs exim -Mrm

#### To check the no. of frozen mails:

exiqgrep -z -c

#### To check exim logs:

tail -f /var/log/exim_mainlog

#### Force delivery of one message:

exim -M mail_id

#### Force another queue run:

exim -qf

#### Force another queue run and attempt to flush frozen messages:

exim -qff

#### To check if there are frozen emails:

exim -bp |awk '/fr[o]zen/ {print}'

#### To clear just one email:

exim -Mrm mail_id

#### Check the subjects of the emails:

 exiqgrep -i |awk '{ print "exim -Mvh "$1 }' |sh |grep -i Subject

#### Clear the mail queue:

cd /var/spool/exim/
rm -rf /var/spool/exim/{input,db}
exim -bpru|awk {'print $3'}|xargs exim -Mrm

#### Deleting mails in the queue that are older than 5 mins perhaps.

exiqgrep -o 300 -i | xargs exim -Mrm

/usr/sbin/exim -v -Mrm (MAIL ID HERE) ==> REMOVE MAILS BY ID

/usr/sbin/exim -bp ==> LIST QUEUED MAILS

/usr/sbin/exim -bpc ==> OUTPUT NUMBER OF QUEUED MAILS

/usr/sbin/exim -bpr | grep ‘*** frozen ***’ | awk ‘{print $3}’ | xargs exim -Mrm ==> DELETE FROZEN MAILS

/usr/sbin/exim -qff -v -C /etc/exim.conf & ==> DELIVER FORCEFULLY EMAILS

/usr/sbin/exiqgrep -i -f (MAIL ADDRESS HERE) | xargs exim -Mf ==> FREEZE MAILS FROM SENDER

/usr/sbin/exiqgrep -i -f (MAIL ADDRESS HERE) | xargs exim -Mrm ==> REMOVE MAILS FROM SENDER

exim -M email-id ==> Force delivery of one message.

exim -qf ==> Force another queue run.

exim -qff ==> Force another queue run and attempt to flush the frozen message.

exim -Mvl messageID ==> View the log for the message.

exim -Mvb messageID ==> View the body of the message.

exim -Mvh messageID ==> View the header of the message

exim -Mrm messageID ==> Remove message without sending any error message

exim -Mg messageID ==> Giveup and fail message to bounce the message to the Sender


exim -bpr | grep "<" | wc -l ==> How many mails on the Queue?

exim -bpr | grep frozen | wc -l ==> How many Frozen mails on the queue



#### Email activity from a particular source :

grep "cwd=" /var/log/exim_mainlog|awk '{for(i=1;i<=10;i++){print $i}}'|sort|uniq -c|grep cwd|sort -n

#### Finding compermised email accounts :

egrep -o 'dovecot_login[^ ]+' /var/log/exim_mainlog | sort|uniq -c|sort -nk 1

#### Few commands to check spaming while it is in action.

#### To get a sorted list of email sender in exim mail queue. It will show the number of mails send by each one.

 exim -bpr | grep "<" | awk {'print $4'} | cut -d "<" -f 2 | cut -d ">" -f 1 | sort -n | uniq -c | sort -n

#### You will get a result as like follows,

```
      1  arun@testdomain.com
      2  sales@test1domain.com
      3  sandy@test123.com
      4  root@testdomain.co.in
    29  admin@testdomain.in
  124  arun@test123domain.com
```


#### The following scripts will check the script that will originate spam mails:

grep "cwd=/home" /var/log/exim_mainlog | awk '{for(i=1;i<=10;i++){print $i}}' | sort | uniq -c | grep cwd | sort -n

awk '{ if ($0 ~ "cwd" && $0 ~ "home") {print $3} }' /var/log/exim_mainlog | sort | uniq -c | sort -nk 1

grep 'cwd=/home' /var/log/exim_mainlog | awk '{print $3}' | cut -d / -f 3 | sort -bg | uniq -c | sort -bg

#### You will get a result as like follows for the first two scripts. The third script is just a sub of the first two scripts.

 ```    
     9      cwd=/home/test1/public_html
     10     cwd=/home/test2/public_html/a1/www
     15     cwd=/home/test3/public_html
     91     cwd=/home/test4/public_html
    178    cwd=/home/test5/public_html/web
    770    cwd=/home/test6/public_html/foro
    803    cwd=/home/test7/public_html/web
124348 cwd=/home/test8/public_html/wp/wp-content/themes/twentyeleven
```


#### If we need to find out exact spamming script. The following script will shows the current spamming script running now. The following script will help you in all time of mail servers. It will help you to find the exact script which sending mails.

 ps auxwwwe | grep <user> | grep --color=always "<location of script>" | head

#### The usage of the above script is as shown below.

 ps auxwwwe | grep test8 | grep --color=always "/home/test8/public_html/wp/wp-content/themes/twentyeleven" | head

#### Once you find the exact script, the following script will help you to find the IP address which is responsible for spamming. You will get a list of IPs from the following script. The IPs address which has high number of access is most probably causing spamming. You can block the IP address in csf or apf firewall.

 grep "<script_name>" /home/user/access-logs/testdomain.com | awk '{print $1}' | sort -n | uniq -c | sort -n



#### Following command that will show you the script which is using script to send the email. If it is from php then use


 egrep -R "X-PHP-Script"  /var/spool/exim/input/*



#### It shows top 50 domains using mail server with options.

 eximstats -ne -nr /var/log/exim_mainlog



#### It shows from which user's home the mail is going, so that you can easily trace it and block it if needed.it shows the mails going from the server.

 ps -C exim -fH ewww | grep home


#### It shows the IPs which are connected to server through port number 25. It one particular Ip is using more than 10 connection you can block it in the server firewall.

 netstat -plan | grep :25 | awk {'print $5'} | cut -d: -f 1 | sort | uniq -c | sort -nk 1


#### In order to find "nobody" spamming, issue the following command

 ps -C exim -fH ewww | awk '{for(i=1;i<=40;i++){print $i}}' | sort | uniq -c | grep PWD | sort -n

#### It will give some result like:
#### Example :

6 PWD=/
347 PWD=/home/sample/public_html/test
Count the PWD and if it is a large value check the files in the directory listed in PWD
(Ignore if it is / or /var/spool/mail /var/spool/exim)

#### The above command is valid only if the spamming is currently in progress. If the spamming has happened some hours before, use the following command.


 grep "cwd=" /var/log/exim_mainlog | awk '{for(i=1;i<=10;i++){print $i}}' | sort | uniq -c | grep cwd | sort -n



#### The following script will give the summary of mails in the mail queue.

exim -bpr | exiqsumm -c | head


#### You will get a result as like follows,

```
Count  Volume  Oldest  Newest  Domain
-----      ------      ------     ------       

  114   171KB     24h     28m  testdomain.com
   15    28KB       36h     7m    gmail.com
    5     10KB       34h     10h   test2domain.com
    4     8192        27h     4h     yourdomain.com
    4     75KB       7m      7m    server.domain.com
    3     6041        23h     42m  test123.com
 ```
