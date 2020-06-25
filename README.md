# RedHat
Script To change password all user on the machines Over Network
#!/bin/bash

FILE=ip machine.txt

MyServer=""
MyUser=""
MyPassword=""
MyPort=""

exec 3<&0
exec 0<$FILE

while read line
do
    MyServer=$(echo $line | cut -d'|' -f1)
    MyUser=$(echo $line | cut -d'|' -f2)
    MyPassword=$(echo $line | cut -d'|' -f3)
    MyPort=$(echo $line | cut -d'|' -f4)

    HOST=$MyServer
    USR=$MyUser
    PASS=$MyPassword

        sshpass -p $PASS ssh -p $MyPort -o StrictHostKeychecking=no $USR@$HOST \
            -T "echo 'T@nm3y@H' | passwd --stdin root"                 \
            < /dev/null | tee -a output.log
done

exec 0<&3
