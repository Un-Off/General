# Networking
    ip link show  (ip l)
    ip address show (ip a)

    cat /etc/resolv.conf
    ls /etc/sysconfig/network-scripts/
    cat /etc/sysconfig/network-scripts/ifcfg-enp0s3
        BOOTPROTO=dhcp (network is dynamically configured, none if statically)

    (network manager)
    nmtui  (terminal user interface)
    nmcli  (command line interface)

    /etc/hosts (static dns)
    192.168.1.82 db server

## Configure network services to start automatically at boot
    sudo dnf install NetworkManager
    sudo dnf enable --now NetworkManager
    systemctl status NetworkManager.service

    nmcli connection show
    sudo nmcli connection modify enpos3 autoconnect yes

## Start, stop, and check the status of network services
    netstat (old), ss(morden)
    sudo ss -ltunp (list listening, tcp, udp, numeric(port number), process )
    systemctl status chronyd.service
    systemctl stop chronyd.service
    systemctl status sshd.service

    sudo ss  -ltunp
    ps <processID>
    sudo lsof -p <processID>

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
    (advised to add the temporary service and make it permanent)

## Routing
    route (Kernel IP routing table)
    ip rout add <IP> via <Gateway>

### Statically route IP traffic
    A in net1 can't talk to B in net2
    let's add C between net1 and net2 (C is a router)
    A can't communicate B, because it do not know how to send it to B
    we can do it my adding network rout

    ip rout add 192.168.0.0/24 via 10.11.12.100 <deviceName> <interfaceName>
    ip rout add 192.168.0.0/24 via 10.11.12.100 dev             enp0s3
    ip rout del 192.168.0.0
    ip rout add default via 10.11.12.100 dev             enp0s3
    ip rout del default via 10.11.12.100 dev             enp0s3

    (to make the connection on boot)
    nmcli connection show
    sudo nmcli connection modify enp0s3 +ipv4.routes "192.168.0.0/24 10.0.0.100"
    sudo nmcli device reapply enp0s3
    (to removr)
    sudo nmcli connection modify enp0s3 -ipv4.routes "192.168.0.0/24 10.0.0.100"
    sudo nmcli device reapply enp0s3

## Synchronize time using other network peers

    systemctl status chronyd.service
    timedatectl
    (if above don't give logs, follow the bellow steps)
    sudo timedatectl set-timezone <timezone = region/city>
    timedatectl list-timezones

    sudo dnf install chrony
    sudo systemctl start chronyd.services
    sudo systemctl enable chronyd.services
    timedatectl
    sudo timedatectl set-ntp true



## Switching
    ip link
    ip addr add <IP> dev ath0

		
## Deafult Gateway
	ip rout deafault <IP> via <Gateway>
		
	Forwarding - /proc/sys/net/ipv4/ip_forward
	

	 