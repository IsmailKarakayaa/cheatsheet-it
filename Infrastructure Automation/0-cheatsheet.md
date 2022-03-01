# Cheat sheets and checklists

- Student name: Ismail Karakaya
- Github repo: <https://https://github.com/HoGentTIN/infra-2122-IsmailKarakayaa>

## Basic commands

| Task                | Command |
| :---                | :---    |
| Query IP-adress(es) | `ip a`  |

## Vagrant Commands
| Command | Task |
| :---    | :--- |
|`vagrant status`| Geeft de staat weer van de machines die beheerd worden door Vagrant.|
| `vagrant up` | Creeert en configureert de guest machine volgens je Vagrantfile. Start de guest machine op als het al gecreeerd en configureerd is. |
| `vagrant halt` | Schakelt de draaiende machine uit die beheerd wordt door Vagrant.|
| `vagrant destroy ` | Verwijdert de machine die beheerd wordt door Vagrant.|
| `vagrant ssh [name]` | Creeert een SSH sessie om shell access te krijgen binnen een draaiende virtuele machine.|
| `vagrant box add --version v202107.28.0 bento/ubuntu-20.04` | Fixt de errors die optreden bij `vagrant up`. Command gegeven in klas. |

## Docker commands
| Command | Task |
| :---    | :--- |
| `docker --help` | Geeft een lijst weer van docker commando's |
| `docker [command] --help` | Geeft informatie weer over het gegeven commando met aanvullende opties die je kan gebruiken.|
| `systemctl status docker` | Geeft de status van de Docker engine weer.|
| `docker ps` | Geeft een lijst weer van draaiende Docker containers. |
| `docker ps -a` | Geeft een lijst weer van alle Docker containers. |
| `docker images` | Geeft een lijst weer van Docker images.|
| `docker volume ls` | Geeft een lijst weer van de volues binnen Docker.|
| `sudo ss -tlnp` | Geeft de netwerk TCP server poorten weer die gebruikt worden op het systeem. |
| `docker run [NAME]` | Creeert een container van de gegeven image. |
| `docker stop [NAME]`| Stopt een draaiende container. |
| `docker rm [name \| container id]` | Verwijdert de Docker image van het systeem. |
| `docker run -i -t --name [name] [imagename]` | Creeert een container **interactief** (`-i`) en opent een **shell** (`-t`)  |
| `docker run -i -t -d --name [name] [imagename]` | Creeert een **detached** (`-d`) container in achtergrond. |
| `docker inspect [name]` | Geeft een zeer detailleerd overzicht weer van een container met de configuraties.|
| `docker top [name]` | Geeft de running processes weer van een container.|
| `docker exec [OPTIONS] [CONTAINER] [COMMAND]`  | Voer een commando uit binnen in running container. Handig als de container gerunned wordt in detached mode.  |
| `docker exec -t [CONTAINER] /bin/hostname` | Geeft de hostname weer van de machine.|
| `docker exec -t [CONTAINER] /sbin/ip a` | Geeft de netwerk interfaces weer van de machine. |
| `docker exec -i -t [CONTAINER] /bin/sh` | Gaat in de shell van de container. |

## Labo 3: Troubleshooting

| Command | Task |
| :---    | :--- |
| `ip link` | Display the configuration of network interfaces/devices.|
| `ip address` | Display the address configuration of network interfaces/devices.|
| `ip route` | Display default gateway configuration and routing table.|
| `cd /etc/sysconfig/network-scripts/`| Navigate to the folder network-scripts for network interface scripts and files.|
| `sudo journalctl -f` | Watch the logs. |
| `systemctl restart network` | Restart the network services. |
| `cat /etc/resolv.conf`| Display DNS server configurations. |
| `ping 192.168.56.72` | Ping a desired host system. |
| `dig icanhazip.com` | Check connectivity to your desired DNS server.|
| `nslookup icanhazip.com` | Check connectivity to your desired DNS server. |
| `getent ahosts icanhazip.com` | Check connectivity to your desired DNS server.|
| `systemctl list-units --type=service` | Display the active services on your system|
| `systemctl list-units --type=service --all` | Display all the services (including disabled) on your system. | 
| `systemctl status [NAME]` | Display the status of a desired service on your system. |
| `systemctl start [NAME]` | Start a desired service.|
| `ss -tlnp` | Display network socket related information. In this case: (TCP, listening ports, port numbers, processes) |
| `ss -ulnp` | Display network socket related information. In this case: (UDP, listening ports, port numbers, processes) |
| `tree /etc/NAME/` | List the directory structure of a desired service. Most of the times the config file you need to edit is located in a conf directory. There you can correct the port number when needed. |
| `systemctl restart [NAME] ` | Restart a desired service to apply changes made after editing the conf file of a service. |
| `sudo firewall-cmd --list-all`  | Display the firewall rules of your system. |
| `sudo firewall-cmd --add-service [NAME] --permanent` | Permanently add a desired service to the firewall rules to allow traffic. |
| `sudo firewall-cmd --reload` | Restart the firewall to apply any changes. |
| `sestatus` | Display the status of SELinux. | 
| `ls -lZ /var/www/html` | Check SELinux file context. |
| `sudo restorecon -R /var/www/` | Set file context to default value of desired directory. |
| `sudo chcon -t httpd_sys_content_t test.php` | Set file context to specified value of the test.php file. (Desired file possible) | 
| `getsebool -a | grep [NAME]` | Get the SE booleans of a desired service. |
| `sudo setsebool -P [BOOL-NAME] on` | Permanently enable a SEboolean of a desired service. |

## Git workflow

Simple workflow for a personal project without other contributors:

| Task                                         | Command                   |
| :---                                         | :---                      |
| Current project status                       | `git status`              |
| Select files to be committed                 | `git add FILE...`         |
| Commit changes to local repository           | `git commit -m 'MESSAGE'` |
| Push local changes to remote repository      | `git push`                |
| Pull changes from remote repository to local | `git pull`                |

## Checklist network configuration

1. Is the IP-adress correct? `ip a`
2. Is the router/default gateway correct? `ip r -n`
3. Is a DNS-server available? `cat /etc/resolv.conf`

