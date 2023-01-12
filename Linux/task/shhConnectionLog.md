## bash script
    ```bash
    #! /bin/bash
    names=( "saran" "sneha" )
    ips=("10.10.20.150" "10.10.20.149")
    pass=( "root@123" "root@123" )
    n=2 #numberofservers
    date >> db.txt
    for (( i = 0 ; i < n ; i++))
    do
        sshpass -p '${pass[i]}' ssh ${names[i]}@${ips[i]}
        echo "${names[i]}@${ips[i]}         $?" >> db.txt
    done
    ```