## subscription-manager
    sudo subscription-manager register --username=<username> --password=<password>
    sudo subscription-manager attach auto

    sudo yum repolist
    sudo yum repolist -v
    sudo yum repolist all
    sudo subscription-manager repos --enable <repo_name>
    sudo subscription-manager repos --disable <repo_name>
    (subscription-manager - yum-config-manager)

## Repo
    sudo yum install yum-utils
    sudo yum-config-manager --add-repo <repo_name or ip/name>
    sudo yum search 'wed server'

    yum install nginx
    yum reinstall nginx
    yum remove nginx

    yum group list (--hidden)
    yum group install 'server with GUI'
    yum group remove 'server with GUI'

    sudo wget http://.../nomachine_version_.rpm
    sudo yum install ./nomachine_version_.rpm
    sudo yum remove nomachine
    sudo yum autoremove
    sudo yum check-upgrade

## To install sshpass
    yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
    yum install sshpass
    man sshpass


