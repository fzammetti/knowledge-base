# Linux

## User/Group Management

### List all users

    cat /etc/passwd

### Show my user ID

    whoami

### Show groups I’m a member of

    groups

### Show details about me

    id

### Add group

    addgroup

### Add user or add user to group

    adduser

### Delete user or remove user from group

    deluser

### Change my password

    passwd

### Show what commands I can sudo

    sudo -l

### Give user ability to run all commands with sudo

* Switch to root, then run **visudo**
* Find where it says **root ALL=(ALL) ALL**
* Type "o" to insert a new line below it
* Now type what you want to insert, like: **\<username> ALL=(ALL) ALL** (use tabs instead of spaces)
* Hit ESC to exit insert-mode
* Type **:x** to save and exit

### Switch user

    su

### Super user do (run as root)

    sudo

### Display user info

    id <username, optional>

## File/Disk Management

### List directory contents

    ls

### Show hidden files and directories

    ls -al

### Probably the most useful version of ls for general purposes

    ls -lha

### Delete directory that isn’t empty

    rm -r XXX

### Show free space

    df -h

### Display text file contents

    cat

### Change directory

    cd

### Show current directory

    pwd

### Clear console

    clear

### Remove a file or directory

    rm

### Create new empty file or change timestamp of existing file

    touch

### Watch a file as it changes

    tail

### Search a file for text string

    grep

Examples:

* Search for a string inside a given file:

      grep <string> <filename>

* Search for a string inside all files in a directory:

      grep <string> *

* Search for a string in all files in a directory with the subdirectories recursed:

      grep -r <string> *

* Search for a string (case insensitive):

      grep -i <string> <file>

To any of these, you can add *| {less|more}* to the end for better viewing.

### Change group owner of a file or directory

    chgrp

### Change permissions of a file or directory

    chmod

### Change user owner of a file or directory (can change group too)

    chown

### Edit a text file

* mcedit
* joe
* nano

(or **vi** or **emacs*, of course, but I prefer any of those others, in the order shown above)

### Display rights of a file in numeric format

    stat -c %a <file_or_directory_name>

### Mount a Windows share

* First, make sure Samba and CIFS-utils are installed
* Then, add an entry to */etc/fstab*:

        //<unc_path_to_share> /mnt/<name_of_mount_point> cifs username=<username>,password=<password> 0 0

* Then, ensure the mount point is created (i.e., **mkdir /mnt/\<name_of_mount_point>**)
* Execute **mount -a** to add the mount point, then it should work

### To ensure a Windows share is reconnected at boot, add an entry to **/etc/fstab**

    //<hostname>/<sharename>   <mount_point>    cifs    username=XXX,password=YYY 0 0

### Generate an RSA keypair

    ssh-keygen -t rsa -b 4096
 
## Environment

### List all commands you could run

    compgen -c

### List all aliases you could run

    compgen -a

### List all built-ins you could run

    compgen -b

### List all keywords you could run

    compgen -k

### List all functions you could run

    compgen -A

### List all the above in one go

    compgen -A function -abck

### Show processes listening on ports

    netstat -tupln

### Process info
* List currently running: **ps**
* Stop a process: **kill**
* Find out about all running processes: **ps -ag**
* Process manager: **top**

### Get status of a service

    service <service_name> status

### Show shell history

    history

### Clear ALL shell history

    history -c

### Clear one specific history item

    history -d <xxx>

### List all packages installed directly and runnable

Assuming an APT-based system (like Ubuntu), you can do this in two steps.  First, do:

    apt install aptitude

...and then...

    aptitude search '~i!~M'

### View systemd logs

    journalctl

## Shell Scripting

### Referring to arguments

    $1, $2

...and so on. Note: **$0** is your script name.

### Conditional statement

    if {condition} then ... fi

### Number comparison

    -eq, -ne, -lt, -le, -gt, -ge

### Test for file properties

* **-s \<filename>** tells you if the file is not empty
* **-f \<filename>** tells you whether the argument is a file and not a directory
* **-d \<filename>** tests if the argument is a directory, and not a file
* **-w \<filename>** tests for writeability
* **-r \<filename>** tests for readability
* **-x \<filename>** tests for executability

### Boolean logic operators

* **!** tests for logical not
* **-a** tests for logical and
* **-o** tests for logical or

### Multilevel conditional

    if {condition} then {statement} elif {condition} {statement} fi

### For loop

    for {variable name} in {list} do {statement} done

### While loop

    while {condition} do {statement} done

### Case statement

    ase {variable} in {possible-value-1}) {statement};; {possible-value-2}) {statement};; esac

### Read keyboard input

    read {variable-name}

### Define a function

    function-name() { #some code here return }

## CentOS-Specific (probably... mostly... kinda... sorta...)

### Get active firewall zone

    firewall-cmd --get-active-zones

### List firewall services on a zone

    firewall-cmd --zone=<zone> --list-all

### Add a firewall rule

    firewall-cmd --permanent --zone=<zone> --add-port=<port>/<type=tcp|udp>

### Restart firewall after changes

    systemctl restart firewalld.service

### Setting up a mount point to a Windows share

    yum install samba samba-client cifs-utils
    mkdir /mnt/<mount_point>
    mount.cifs //<server_name>/<share_name> <mount_point> -o user=<username> pass=<password>

### Yum manual installation

If files can’t be downloaded by yum for some reason, you can manually download them and then put them in:

    /var/cache/yum/x86_64/7/*/packages

Yum will use them as if it had downloaded the RPM itself.

### Set default system text editor

    echo "set nowrap" >>/etc/nanorc
    cat <<EOF >>/etc/profile.d/nano.sh
    export VISUAL="nano"
    export EDITOR="nano"
    EOF

### Change default version of Python

    alternatives --install /usr/bin/python python /usr/bin/python3.6 2
    alternatives --install /usr/bin/python python /usr/bin/python2.7 1

Assuming you have Python2 and Python3 installed, and they are at those versions, setting Python3 to a higher priority (2) makes it default.  Now, **python –version** will show 3.6.

### Add EPEL repo for extra apps

    yum install epel-release

### Doing yum update shows “This system is not registered with an entitlement server. You can use subscription-manager to register.”

How to get rid of it:

    mcedit /etc/yum/pluginconf.d/subscription-manager.conf
    Set enabled=0
 
## Services and tasks

### Enable a service to start on boot

    systemctl enable <service_name>

### Disable a service from starting on boot

    systemctl disable <service_name>

### See if a service is configured to start on boot

    chkconfig <service_name>

### List all services

    systemctl

### List only running services (or another state)

    systemctl –state=<state>

### To add a cron job

    crontab -e

Add entry, for example:

    0 5 * * * /~/backup_process.sh > /~/backup_process.log

(that will execute the specified shell script at 5am every day with output to the specified log file)

### To back up all jobs

    crontab -l > my_cron_backup.txt

### To restore from backup

    crontab my_cron_backup.txt
    crontab -l

### To list all jobs

    crontab -l

### List enabled units

    systemctl list-unit-files | grep enabled

### View currently active targets

    systemctl list-units --type target

### Create unit file in /etc/systemd/system named xxx.service, example

    [Unit]
    Description=Start all containers at boot
    After=timers.target
    [Service]
    Type=idle
    ExecStart=/root/start_all.sh
    TimeoutStartSec=0
    [Install]
    WantedBy=default.target

Enable unit by reloading systemd daemon:

    systemctl daemon-reload

Enable new service to start automatically at boot:

    systemctl enable xxx.service

Start the service:

    systemctl start xxx.service

Reboot and confirm:

    systemctl reboot
 
## Miscellaneous

### Vi quick reference:

* Remember that you start out in normal mode.  Press **i** to enter insert mode, where you can then type.
* Hit ESC to get back into normal mode.
* In normal mode, commands are triggered by hitting **:** first (which moves cursor to bottom of screen) and then some command like **q** or **i*.

### Important Locations

* User list: **/etc/passwd**
* User passwords: **/etc/shadow**
* Group list: **/etc/group**

## How to verify the integrity of a directory full of JPG files and only show those that aren't uncorrupted

    jpeginfo -c -v /<directory_name>/*.jpg | awk '$0 !~ /\[OK\]/'

### Read mail:

    mail

### Delete all mail (when inside mail already):

    delete *

### Sometimes, copying a file from Windows to Linux will cause problems with line terminators.  To fix:

    vi /<filename>
    :set ff=unix
    :wq

Or, just use dos2unix if installed.

### How to send an email from the command line, low-level:

    telnet <server – can be localhost> 25
    HELO <domain>
    MAIL FROM: <address>
    RCPT TO: <address>
    DATA
    Test

### Download an entire site with wget:

    wget --recursive --no-clobber --page-requisites --html-extension --convert-links --restrict-file-names=windows --no-parent --domains <domain_name> <website_url>
