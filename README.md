# Test
    Commands
	Open FIle:
	lsof -u user | awk '{print $2}' | sort | uniq -c | sort -n -- To check the number of open files per process by user
	/etc/security/limit.conf ---- To set the number of open files
	* soft nofile 65535
    * hard nofile 65535
	To check see /proc/pid/limits
	
	pam_tally --user=aj90610 --reset -- If user account is expired 
	
	net user username /domain --- To check the AD user login & password expiration details

	 ps -eo pcpu,pid,user,args | sort -r -k1 | less
	 
	 
#!/bin/bash
#CPU
hostname=`hostname`
trigger=20.00
load=`top -b -n3 | awk '/^Cpu/ {print $2}' | sed '3!d' `
response=`echo | awk -v T=$trigger -v L=$load 'BEGIN{if ( L > T){ print "greater"}}'`
if [[ $response = "greater" ]]
then
top -b -n2 | mail -s "High CPU load on $hostname - [ $load ]" aj90610@amway.com
fi
#Memory
hostname=`hostname`
trigger=20.00
load=`free -m | awk 'NR==2{printf "%.2f%%\t\t", $3*100/$2 }' `
response=`echo | awk -v T=$trigger -v L=$load 'BEGIN{if ( L > T){ print "greater"}}'`
if [[ $response = "greater" ]]
then
free -m | mail -s "High Memory load on $hostname - [ $load ]" aj90610@amway.com
fi
#Disk
hostname=`hostname`
trigger=10.00
load=`df -h | awk '$NF=="/"{printf "%s\t\t", $5}' `
response=`echo | awk -v T=$trigger -v L=$load 'BEGIN{if ( L > T){ print "greater"}}'`
if [[ $response = "greater" ]]
then
du -sh /* | mail -s "High Disk Usage on $hostname - [ $load ]" aj90610@amway.com
fi

curl -H "Content-Type: application/json" POST -d  http://10.128.200.113:8080/opssupport/api/file/mnt/static/index.html
curl -I -H "Accept: application/json" -H "Content-Type: application/json" -X GET http://localhost:8080/opssupport/api/file/mnt/static/index.html

