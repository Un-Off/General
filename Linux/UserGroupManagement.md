## Create, delete, and modify local user accounts
    sudo useradd john (will create user john and group john, /home/john, /bin/bash \, /etc/skel)
    ls -a /etc/skel
    useradd --defaults
    cat /etc/login.defs 
     
     sudo passwd john (help to set password for john)
     sudo userdel john (--remove will remove john home dir too)

     sudo useradd --shell /bin/othershell --home-dir /home/otherdir/ john (or)
     sudo useradd -s /bin/othershell -d /home/otherdir/ john

     cat /etc/password
     sudo useradd --uid 1100 smith (-u)
     ls -ln /home/ (n numeric userID groupID)

     id, whoami
     sudo useradd --system sysacc

     usermod --home /homeotherdir --move-home john
     usermod -d /homeotherdir --m john

     usermod --login jane john (-l)
     usermod --shell /bin/othershell jane (-s)

     sudo usermod --lock jane (-L . user may still able to login with ssh )
     sudo usermod --unlock jane (-U)
     sudo usermod --expiredate 2022-12-10 jane (-e . use past date to expire immediately. YEAR-MONTH-DAY)

    sudo chage --lastday 0 jane (-d . expire password. next time jane login should chage password)
    sudo chage -d -1 jane (to cancel the above)
    sudo chage --maxdays 30 jane (-M . request to change pass every 30 days)
    sudo chage -M -1 jane (password never expires)

## Create, delete, and modify local groups and group memberships
    sudo useradd john
    sudo groupadd developers
    sudo gpasswd --add john developers (-a)
    group john
    sudo gpasswd --delete john developers (-d)

    sudo usermod -g developers john (--gid . change primary group)
    group john

    sudo groupmod --new-name programmers developers (-n)
    sudo groupdel programmers (primary group can't deleted. chage it and delete it.)

## Manage system-wide environment profiles
    enc (printenv)
    HISRSIZE=2000
    history

    echo $HOME (/home/aaron)
    touch $HOME/saved_file = touch /home/aaron/saved_file

    cat .bashrc 
    sudo vim /etc/environment
    sudo vim /etc/profile.d/lastlogin.sh

    sudo vim /etc/skel/README
    sudo useradd trinity
    sudo la -a /home/trinity
    sudo vim /home/trinity/.bashrc
        PATH="$HOME/.local/bin:$HOME/bin:/opt/bin:$PATH (:/opt/bin:)
    echo $PATH
    specialtool (/opt /bin /specialtool)

## Configure user resource limits
    sudo vi /etc/security/limits.conf
    #<domain> <type> <item>  <value>
    user_name, @group_name, * (all)
    soft, hard, - (hard and soft)
    nproc (max number of processors), fsize, cpu
    man limits

    sudo -iu trinity (i login, u user)
    ulimit -a

## Manage user privileges
    sudo gpasswd -a trinity wheel
    sudo gpasswd -d trinity wheel

    sudo visudo

    sudoers
        trinity ALL=(ALL)  ALL
        %developers ALL=(ALL)  ALL
        trinity ALL=(aaron, john)  ALL
        trinity ALL=(aaron, john)  ALL
        trinity ALL=ALL
        trinity ALL=(ALL) /bin/ls, /bin/stat
        trinity ALL= NOPASSWD:ALL

## Manage access to the root account
    sudo --login, sudo -i (user with sudo can login as root)
    su -, su -l, su --login (login as root user with passwd)

    when su is locked dwe can still use sudo --login but su - is not allowed
    sudo passwd --unlock root (-u)
    sudo passwd --lock root (-l)

## Configure PAM
    plugable authentication module
    su

    ls /etc/pam.d
    sudo vim /etc/pam.d/sudo
    man pan.conf



