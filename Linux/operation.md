## Boot, reboot, and shutdown a system safely
    systemctl reboot
    systemctl poweroff
    systemctl shutdown +15  'shutdown in 15mins' (-r reboot)
    (--force, --force --force)

    systemctl get-default (target)
    systemctl set-default multi-user.target
    systemctl isolate graphic.target (emergency rescue)

## Scripting
    #!/bin/bash   shebang

    date >> /tmp/script.log
    cat /proc/version >> /tmp/script.log


    if test -f /tmp/archiver.tar.gz; then
        mv /tmp/archiver.tar.gz /tmp/archiver.tar.gz.old
        tar acf /tmp/archiver.tar.gz /etc/dnf
    else
        tar acf /tmp/archiver.tar.gz /etc/dnf
    fi

## Manage the startup process and services (In Services Configuration)
    init system (initialization)
    UNITS - service, socket, device, timer.

    systemctl   __________  sshd.service
                cat
                edit -full 
                revert 
                status 
                stop 
                start 
                restart 
                reload
                reload-or-restart
                disable
                is-enable
                enable (auto start on boot)
                enable --now (enable and start)
                disable --now (disable and stop)
                mask (will not allow services to start other services)
                unmask
                list-units --type services --all

## Diagnose and manage processes
    man ps
    ps 
    ps aux
    top
    ps 1
    ps u 1
    ps -U aaron (process started by specific user)
    ps u -U aaron 
    pgreb -a syslog
    nice -n 11 bash (nice value priority to take cpu time)
    ps l (l show NI column nice value)
    ps lax
    ps fax (tree like parent child process)
    ps faux
    renice <new_nice_value> <processID>
    
    kill -L
    ctrl + z (pause app)
    fg  (get back to foreground)

    sleep 300 & (back grounding the process)
    jobs (list bg )

    pgrep -a bash
    lsof -p <processID>(list open files)
    lsof -p 1

## Locate and analyze system log files
    rsyslog = rocket-fast system for log processing
    ls /var/log/

    grep -r 'ssh' /var/log/
    less /var/log/secure
    less /var/log/messages
    which sudo    (find path to sudo)
    journalctl /bin/sudo   
    journalctl -u sshd.service   
    journalctl -f  (follow mode)
    journalctl -p  (tab tab) (info,warning,err,crit,etc..)
    journalctl -p info -g '^b' (lines begin with b)
    journalctl -S 02:00 (-S since)
    journalctl -S 01:00 -U 02:00  (-U until)
    last 
    lastlog

## Schedule tasks to run at a set date and time

    crontab -e (-e edit, -l list, -r remove entirely, -u username )

    sudo vim /etc/anacrontab

    at 'now + 30 minutes' ('2:30 August 20 2022')
    path_to_command file_created_by_at
    atq
    at -c 20























