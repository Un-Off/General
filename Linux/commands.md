man man

## Files
    aprops director
    cd (takes u to home dir)
    cd (takes u to previous dir)

    cp -r (recursive all file in the dir to another)
    rm (remove)
    mv


## Links
    hard link (points to inode of a file)
    stat <file.name>
    ln target_file link_file

    can't hard link to files in different filesystem
    Hard links are not allowed for directories.(Only a superuser* can do it)
    Hard links are comparatively faster.

    soft link (points to file path)
    ln -s target_file link_file

## file permissions
    chgrp group_name file/dir
    groups (list of groups)
    chown owner_name file/dir
    chown owner_name:group file/dir (to change both owner and group)

    d - dir
    - - regular file
    c - character device
    l - link
    s - socket file
    p - pipe
    b - block device

    chmod u+rwx,g-rw,o=w

### SUID
    chmod 4664 file.name (executes as the user who owns the file)
    -rwSrw-r3--
    chmod 4764 file.name
    -rwsrw-r3--  (s = SUID + Execute)

### SGID
    chmod 2664 file.name
    -rw-rwSr3--
    chmod 2674 file.name
    -rw-rws-r3--
### sticky bit
    only the user who owns the file remove it
    chmod 1777 file.name
    -rwxrwxrwt
    chmod 1776 file.name
    -rwxrwxrwT

### Search for files
    find /usr/share/ -name `*.jpg` (by name, -name for case insensitive)
    find /lib64/ -size +10M (my size)
    find /dev/ -mmin -1 (last modified min file)
    [note: -cmin change minute will find changes made to metadata like file permissions]
    and :- find /lib64/ -size +10M -name `*.jpg
    or :- find /lib64/ -size +10M -o -name `*.jpg
    not :- find /lib64/ -not -name `*.jpg  ( \! )

    find -perm 644 (-644 least, /644 any,)

## Compare and manipulate file content

    cat
    tar
    head
    tail -n 20 file.name

    sed (stream editor)
    sed 's/canda/canada/g' file.name   (s - search and replace, g- global in all places, preview the file)
    sed -i 's/canda/canada/g' file.name   (i --in-place, edit the file)

    cut (-d delimiter) ' ' (-f field 1 first-word in each line) 

    sort file.name | uniq

    diff file1.name file2.name  (-y or sdiff gives side by side comparison)

## pagers and vi
    pager
    less file.name
    more file.name 

    vi
    yy copy
    p  past
    dd cut
    

    :w write
    :q quit
    :/wordToSearch
    n next

## Search file using Grep

    grep 'wordToSearch' file.name (-i ignore case, -vi invert_match, -w only_words, -o only_matching_words )
    grep -r 'wordToSearch' path_to_dir (-r recursive)

    ^A - start with A
    A$ - end with A
    '.' - any single char ('c.t' - cat is a match)

    egreb
    a{3} match aaa
    {x,} atlest x
    {,x} atmost x
    

## Archive, backup, compress, unpack, and un-compress files
### Archive file

    Archive (backup.tar  tape archive) --> Compress (backup.tar.gz) --> Backup

    tar --list --file archive.tar === tar -tf archive.tar ===  tar tf archive.tar
    cf === --create --file
    rf === --append --file
    xf === --extract --file
    --directory /tmp/ === -C /tmp/  (extract in the specific dir)
### compress file

    gzip , bzip, xz file (will compress and delete the file [--keep]. --decompress or gunzip, bunzip, unxz  )
    caf === --create --autocompress --file
    tar caf achieve.xz file1
    tar xf achieve.tar.gz file1

## Backup files to a Remote System
    rsync === remote synchronization
    rsync -a <source> <destination> or rsync -a <localPath> <IP>:<remotePath> 

    Disk Imaging 
    sudo dd  if=/dev/vda of=diskImage.raw bs=1M status=progress
    if (input file), of (output file), bs (block size), 
    should not do in virtual machine as it overrides 
    
## input and output redirection
    output
    >   (over-write)
    >>  (append)

    > stdin
    1> stdout
    2> stderr

    2>&1 stderr redirect to stdout

    heredoc
        sort <<EOF
    here string
        bc <<1+2+3

    piping
        |

