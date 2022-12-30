# Networking

## Switching
    ip link
    ip addr add <IP> dev ath0
## Routing
    route (Kernel IP routing table)
    ip rout add <IP> via <Gateway>
		
## Deafult Gateway
	ip rout deafult <IP> via <Gateway>
		
	Forwarding - /proc/sys/net/ipv4/ip_forward
	
## Firewall (firewall-cmd)
    --get-deafult-zone
    --set-deafult-zone=public
    --list-all
    --info-service=cockpit (display ports)
    --add-service=http	--add-port=80/tcp
    --remove-service=http	--remove-port=80/tcp
    --add-source=<IP> --zone=trused
    --get-active-zones
    --permanent
	 