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
