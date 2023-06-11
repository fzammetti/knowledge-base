# WSL

---

* [List installed distros](#477d6a83-9c55-484d-8bbf-3a2d1066b138)
* [Export distro](#ad11942b-2825-4109-aaaf-40b6de8d3449)
* [Import distro](#673be136-cdc6-4b3e-b2a7-8a56aeab125c)
* [To start Docker Daemon in WSL2](#3eac9c37-3262-419c-8c2b-789fd508da66)
* [To install Docker in Ubuntu](#73cc3a41-01e0-45bc-8c43-407672e6c1bb)
* [Use cron in WSL](#69a8799f-1563-4858-8ae3-a155b12fa3cc)

---




<div id="477d6a83-9c55-484d-8bbf-3a2d1066b138">

## List installed distros

</div>

    wsl --list –all




<div id="ad11942b-2825-4109-aaaf-40b6de8d3449">

## Export distro

</div>

    wsl --export <distro_name> ./<distro_name>.tar




<div id="673be136-cdc6-4b3e-b2a7-8a56aeab125c">

## Import distro

</div>

    mkdir ~/AppData/Local/<distro_name>
    wsl --import <distro_name> ~/AppData/Local/<distro_name> ./<distro_name>.tar --version 1|2




<div id="3eac9c37-3262-419c-8c2b-789fd508da66">

## To start Docker Daemon in WSL2

</div>

    /etc/init.d/docker start




<div id="73cc3a41-01e0-45bc-8c43-407672e6c1bb">

## To install Docker in Ubuntu

</div>

The official installation instructions here: https://docs.docker.com/engine/install/ubuntu

But, the basic procedure is:

    apt remove docker docker-engine docker.io containerd runc
    apt update
    apt install apt-transport-https ca-certificates curl gnupg lsb-releast
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    apt update
    apt install docker-ce docker-ce-cli containerd.io

Then, start the daemon as described above and run **docker run hello-world** to test




<div id="69a8799f-1563-4858-8ae3-a155b12fa3cc">

## Use cron in WSL

</div>

* Create a shortcut to start cron in WSL when you log in.  Open startup folder:

      Run shell:statup

* Create a shortcut, target:

      C:\Windows\System32\wsl.exe sudo /etc/init.d/cron start

Now you can create cron jobs like always and they'll execute in WSL.
