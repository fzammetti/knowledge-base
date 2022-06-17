# WSL
----------

##  List installed distros

    wsl --list –all

##  Export distro

    wsl --export <distro_name> ./<distro_name>.tar

##  Import distro

    mkdir ~/AppData/Local/<distro_name>
    wsl --import <distro_name> ~/AppData/Local/<distro_name> ./<distro_name>.tar --version 1|2

##  To start Docker Daemon in WSL2

    /etc/init.d/docker start

##  To install Docker in Ubuntu

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
