OS Hardening & ISO update required for Security Scan

Login to all the servers 
su - 
Make sure ISO is Installed
Latest Repo should be available while yum update in order to get security updates
/etc/os-release to check os version

 

In ansible clients
    bash /root/eitools/ansible/ansible_client_setup.sh		(client set-up)
    ss -ltn

 

In ansible master
    /etc/host 
    IP    client_alias

    bash /root/eitools/ansible/ansible_master_setup.sh		(master set-up)

    /etc/ansible/host 
    client_alias prod_ip4=IP

    bash /etc/ansible/scripts/addeihost.sh client_alias	 	(add client to host)

    ansible -m ping SFW  									(check connection between clients-host)

    vi /etc/ansible/group_vars/vars.yml						(edit variables for ansible)
    reposrv :
    ntp_server :
    IPV6 : NO ( depends on ip v in /etc/ansible/host  )

    ansible-vault view /etc/ansible/group_vars/vars.yml 	(view variables in vautl)

    ansible-playbook /etc/ansible/admin/deploy/3-pushEIrepo.yml (push repo)

    ansible-playbook /etc/ansible/admin/deploy/5-secharden.yml   (security hardening)

 

In all servers 
	yum update