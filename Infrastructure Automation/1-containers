# Lab Report: Container Virtualization

## Set up the lab environment
### Setup
First and foremost navigate to the `C:\Users\Ismail\Documents\School\Infrastructue Automation\infra-2122-IsmailKarakayaa\dockerlab` directory and check the status of the dockerlab machine with the following command.

```console
$ cd C:\Users\Ismail\Documents\School\Infrastructue Automation\infra-2122-IsmailKarakayaa\dockerlab
$ vagrant status

dockerlab                 not created (virtualbox)

The environment has not yet been created. Run `vagrant up` to
create the environment. If a machine is not created, only the
default provider will be shown. So if a provider is not listed,
then the machine is not created for that environment.
```

We see that the dockerlab virtual machine has not been created yet. Use the command `vagrant up` to create and start the virtual machine. This can take a while.

```console
$ vagrant up
Bringing machine 'dockerlab' up with 'virtualbox' provider...

[...]

PLAY RECAP *********************************************************************
dockerlab                  : ok=23   changed=9    unreachable=0    failed=0    skipped=19   rescued=0    ignored=0 
```

### Portainer
My dockerlab virtual machine is running a container named portainer. Portainer is a web-ui where we can navigate through our environment and view our stacks, containers, images, volumes, networks,... realtime while working with the CLI. 
Open a web-browser and enter the URL: <http:192.168.56.20:9000>

My username is `admin` and my password is `ismail200`.

![portainer home](img/lab1/0x_portainer_01.PNG)

![portainer local](img/lab1/0x_portainer_02.PNG)

### Docker from the CLI
Now we will explore Docker through the CLI. Login to the dockerlab vm and go to the dockerlab directory.

```console
$ vagrant ssh dockerlab
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-80-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat 09 Oct 2021 03:04:36 PM UTC

  System load:                      0.12
  Usage of /:                       5.9% of 61.31GB
  Memory usage:                     6%
  Swap usage:                       0%
  Processes:                        132
  Users logged in:                  0
  IPv4 address for br-a58951776c3b: 172.30.0.1
  IPv4 address for docker0:         172.17.0.1
  IPv4 address for eth0:            10.0.2.15
  IPv4 address for eth1:            192.168.56.20


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
Last login: Mon Oct  4 10:26:29 2021 from 10.0.2.2
vagrant@dockerlab:~$
```

Let's check the status of the Docker engine.
```console
vagrant@dockerlab:~$ systemctl status docker
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2021-10-09 15:02:22 UTC; 15min ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 964 (dockerd)
      Tasks: 25
     Memory: 129.6M
     CGroup: /system.slice/docker.service
             ├─ 964 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
             ├─1335 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 9000 -container-ip 172.30.0.2 -container-port 9000
             └─1347 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 8000 -container-ip 172.30.0.2 -container-port 8000

Warning: some journal files were not opened due to insufficient permissions.

```

Check the netwerk TCP server ports that are in use on the system.
```console
vagrant@dockerlab:~$ sudo ss -tlnp
State             Recv-Q            Send-Q                         Local Address:Port                          Peer Address:Port            Process
LISTEN            0                 4096                               127.0.0.1:41829                              0.0.0.0:*                users:(("containerd",pid=696,fd=11))
LISTEN            0                 4096                                 0.0.0.0:9000                               0.0.0.0:*                users:(("docker-proxy",pid=1335,fd=4))
LISTEN            0                 4096                              172.30.0.1:9323                               0.0.0.0:*                users:(("dockerd",pid=964,fd=21))
LISTEN            0                 4096                                 0.0.0.0:111                                0.0.0.0:*                users:(("rpcbind",pid=583,fd=4),("systemd",pid=1,fd=107))
LISTEN            0                 4096                           127.0.0.53%lo:53                                 0.0.0.0:*                users:(("systemd-resolve",pid=584,fd=13))
LISTEN            0                 128                                  0.0.0.0:22                                 0.0.0.0:*                users:(("sshd",pid=705,fd=3))
LISTEN            0                 4096                                 0.0.0.0:8000                               0.0.0.0:*                users:(("docker-proxy",pid=1347,fd=4))
LISTEN            0                 4096                                    [::]:111                                   [::]:*                users:(("rpcbind",pid=583,fd=6),("systemd",pid=1,fd=109))
LISTEN            0                 128                                     [::]:22                                    [::]:*                users:(("sshd",pid=705,fd=4))

```

List running Docker containers. Here you will see that a container named `portainer` that is running. This is the container that enables us to have access to our environment using the web-ui portainer. Any future container that you create/run can be displayed using this command. 

```console
vagrant@dockerlab:~$ docker ps
CONTAINER ID   IMAGE                    COMMAND        CREATED      STATUS          PORTS                                                      NAMES
5312746c17e1   portainer/portainer-ce   "/portainer"   6 days ago   Up 17 minutes   0.0.0.0:8000->8000/tcp, 0.0.0.0:9000->9000/tcp, 9443/tcp   portain
```

Now, list the docker images. This will display all the Docker images in our environment. A Docker image is basically a read-only template that comes with instructiong for deploying containers.
```console
vagrant@dockerlab:~$ docker images
REPOSITORY               TAG       IMAGE ID       CREATED       SIZE
portainer/portainer-ce   latest    8377e6877145   2 weeks ago   251MB
```

## Our first containers
### Hello world!

We will run a basic container `hello-world` with the following command. 
```console
vagrant@dockerlab:~$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete
Digest: sha256:9ade9cc2e26189a19c2e8854b9c8f1e14829b51c55a630ee675a5a9540ef6ccf
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```
Our Docker engine noticed that it doesn't have any images that matches the name `hello-world`. It looked for one online in the [Docker Hub](https://hub.docker.com) and downloaded the image. After downloading it, it created/ran a new container based on this image. It printed us a Hello World message.

Let's display our containers first using `docker ps` then `docker ps -a`.
```console
vagrant@dockerlab:~$ docker ps
CONTAINER ID   IMAGE                    COMMAND        CREATED      STATUS          PORTS                                                      NAMES
5312746c17e1   portainer/portainer-ce   "/portainer"   6 days ago   Up 26 minutes   0.0.0.0:8000->8000/tcp, 0.0.0.0:9000->9000/tcp, 9443/tcp   portainer

vagrant@dockerlab:~$ docker ps -a
CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS                     PORTS                                                      NAMES
ecfa308d38c1   hello-world              "/hello"                 6 minutes ago   Exited (0) 6 minutes ago                                                              keen_hoover

5312746c17e1   portainer/portainer-ce   "/portainer"             6 days ago      Up 32 minutes              0.0.0.0:8000->8000/tcp, 0.0.0.0:9000->9000/tcp, 9443/tcp   portainer

```
We see that by using `-a` it displays the `hello-world` the hello-world container whereas the first command doesn't. This happens because the basic display commands only shows us the currently running containers. By using `-a` we can see all our containers: when it was created and exited.

Delete the `hello-world` image using the following. Note that the Docker-engine will return Error response complaining that a container is still using the image. Therefore the container has to be removed by using the ID of the container or the name of the container mentioned in the last column (List ID/name with `docker ps -a`).
```console
vagrant@dockerlab:~$ docker rmi hello-world
Error response from daemon: conflict: unable to remove repository reference "hello-world" (must force) - container ecfa308d38c1 is using its referenced image feb5d9fea6a5

vagrant@dockerlab:~$ docker ps -a
CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS                     PORTS                                                      NAMES
ecfa308d38c1   hello-world              "/hello"                 6 minutes ago   Exited (0) 6 minutes ago                                                              keen_hoover

5312746c17e1   portainer/portainer-ce   "/portainer"             6 days ago      Up 32 minutes              0.0.0.0:8000->8000/tcp, 0.0.0.0:9000->9000/tcp, 9443/tcp   portainer

vagrant@dockerlab:~$ docker rm ecfa308d38c1
ecfa308d38c1

```

### Interactive and detached containers
#### Interactive containers
Running a container interactively means that we can execute commands inside the container while it is still running. We access a command prompt inside the running container. 

Launch an Alpine container **interactively** (`-i`) and open a **shell** (`-t`).
The Alpine Linux is a very small image which is known for it's quick load-up time. 
```console
vagrant@dockerlab:~$ docker run -i -t --name alpine alpine
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
a0d0a0d46f8b: Pull complete
Digest: sha256:e1c082e3d3c45cccac829840a25941e679c25d438cc8412c2fa221cf1a824e6a
Status: Downloaded newer image for alpine:latest
/ #
```

Again, the engine couldn't find the alpine image in our environment. Therefore it downloaded one again from the [Docker Hub](https://hub.docker.com) and ran a container based on this image.
We dropped into a root shell inside the container. Here we can use basic Linux commands and navigate through the instance.

While we are inside the alpine container, let's open a new terminal on our host system. Log into the dockerlab VM.
```console
$ vagrant ssh dockerlab
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-80-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat 09 Oct 2021 04:06:15 PM UTC

  System load:                      0.01
  Usage of /:                       5.9% of 61.31GB
  Memory usage:                     7%
  Swap usage:                       0%
  Processes:                        124
  Users logged in:                  1
  IPv4 address for br-a58951776c3b: 172.30.0.1
  IPv4 address for docker0:         172.17.0.1
  IPv4 address for eth0:            10.0.2.15
  IPv4 address for eth1:            192.168.56.20


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
Last login: Sat Oct  9 15:17:33 2021 from 10.0.2.2

```

List the running containers of the environment.
```console
vagrant@dockerlab:~$ docker container ls
CONTAINER ID   IMAGE                    COMMAND        CREATED         STATUS             PORTS                                                      NAMES
30c0797286aa   alpine                   "/bin/sh"      8 minutes ago   Up 8 minutes                                                                  alpine
5312746c17e1   portainer/portainer-ce   "/portainer"   6 days ago      Up About an hour   0.0.0.0:8000->8000/tcp, 0.0.0.0:9000->9000/tcp, 9443/tcp   portainer
```
We see a container named alpine running currently. This is because we are inside the container on our first terminal.

Use the following command to display detailed low-level information of the alpine container.
```console
vagrant@dockerlab:~$ docker inspect alpine
[
    {
        "Id": "30c0797286aa21214ebf1d7d12e95dc6bb41de4edb5333ae1d10ba387bf5ef23",
        "Created": "2021-10-09T15:58:37.774398452Z",
        "Path": "/bin/sh",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 3154,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2021-10-09T15:58:38.473441981Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:14119a10abf4669e8cdbdff324a9f9605d99697215a0d21c360fe8dfa8471bab",
        "ResolvConfPath": "/var/lib/docker/containers/30c0797286aa21214ebf1d7d12e95dc6bb41de4edb5333ae1d10ba387bf5ef23/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/30c0797286aa21214ebf1d7d12e95dc6bb41de4edb5333ae1d10ba387bf5ef23/hostname",
      
      [...]

       "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "36a9d91e025a19eb45d6d4f7d1fb3243cd7d70d2c6899dba9261abfe4f67277b",
                    "EndpointID": "5cfe5fb82c2eede4c5f5719c8f17bd8c831edaaa91ce1efbd434abadfd1d0c1c",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]


```

Display a list of the running processes of the alpine container.
```console
vagrant@dockerlab:~$ docker top alpine
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                3154                3133                0                   15:58               pts/0               00:00:00            /bin/sh

```

Exit the shell in the Alpine container on the first terminal and repeat the commands. 

List the running containers of the environment. Our container is not running therefore it is not being displayed.
```console
vagrant@dockerlab:~$ docker container ls
CONTAINER ID   IMAGE                    COMMAND        CREATED         STATUS             PORTS                                                      NAMES
5312746c17e1   portainer/portainer-ce   "/portainer"   6 days ago      Up About an hour   0.0.0.0:8000->8000/tcp, 0.0.0.0:9000->9000/tcp, 9443/tcp   portainer
```

 Displaying detailed low-level information of the alpine container. Note that there's a difference. The `"Status"` is flagged as `"exited"` and `"Running"` as `false`.
 ```console
vagrant@dockerlab:~$ docker inspect alpine
[
    {
        "Id": "30c0797286aa21214ebf1d7d12e95dc6bb41de4edb5333ae1d10ba387bf5ef23",
        "Created": "2021-10-09T15:58:37.774398452Z",
        "Path": "/bin/sh",
        "Args": [],
        "State": {
            "Status": "exited",
            "Running": false,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 0,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2021-10-09T15:58:38.473441981Z",
            "FinishedAt": "2021-10-09T16:15:24.449814849Z"
        },
        "Image": "sha256:14119a10abf4669e8cdbdff324a9f9605d99697215a0d21c360fe8dfa8471bab",
        "ResolvConfPath": "/var/lib/docker/containers/30c0797286aa21214ebf1d7d12e95dc6bb41de4edb5333ae1d10ba387bf5ef23/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/30c0797286aa21214ebf1d7d12e95dc6bb41de4edb5333ae1d10ba387bf5ef23/hostname",
        [...]
```

Display a list of the running processes of the alpine container. We will get an error response
```console
vagrant@dockerlab:~$ docker top alpine
Error response from daemon: Container 30c0797286aa21214ebf1d7d12e95dc6bb41de4edb5333ae1d10ba387bf5ef23 is not running

```

#### Detached containers
Running a container means that the container runs in the background of our terminal. 

Launch the alpine container in the background (detached). Note that we will get an Error response that the name already exists. Remove the container first and try again.
```console
vagrant@dockerlab:~$ docker run -i -t -d --name alpine alpine
docker: Error response from daemon: Conflict. The container name "/alpine" is already in use by container "30c0797286aa21214ebf1d7d12e95dc6bb41de4edb5333ae1d10ba387bf5ef23". You have to remove (or rename) that container to be able to reuse that name.
See 'docker run --help'.
vagrant@dockerlab:~$ docker rm alpine
alpine
vagrant@dockerlab:~$ docker run -i -t -d --name alpine alpine
2e6a6ccb132a58da1c4269d5fdf902d3be6aacd9a8314f84698c0226235a1774
vagrant@dockerlab:~$
```

Upon launching the container we get a unique hash which identifies the container. 

Next we will use commands to execute a command inside the Alpine container.

Retrieve the hostname of the alpine container.
```console
vagrant@dockerlab:~$ docker exec -t alpine /bin/hostname
2e6a6ccb132a

```

Retrieve the network addresses of the alpine container.
```console
vagrant@dockerlab:~$ docker exec -t alpine /sbin/ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
16: eth0@if17: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
```

Enter the shell of the alpine container.
```console
vagrant@dockerlab:~$ docker exec -i -t alpine /bin/sh
/ #
```

- Compare te host name with the container ID
```console
vagrant@dockerlab:~$ docker exec -t alpine /bin/hostname
2e6a6ccb132a
vagrant@dockerlab:~$ docker ps
CONTAINER ID   IMAGE                    COMMAND        CREATED         STATUS         PORTS                                                      NAMES
2e6a6ccb132a   alpine                   "/bin/sh"      9 minutes ago   Up 9 minutes                                                              alpine
5312746c17e1   portainer/portainer-ce   "/portainer"   6 days ago      Up 2 hours     0.0.0.0:8000->8000/tcp, 0.0.0.0:9000->9000/tcp, 9443/tcp   portainer
```

the hostname: `2e6a6ccb132a`

the container ID: `2e6a6ccb132a`

- What's the IP address of the container? 
  
  172.17.0.2

- After you exit the shell is the container still running?
  
  Yes

Stop and remove the container.
```console
vagrant@dockerlab:~$ docker stop alpine
alpine
vagrant@dockerlab:~$ docker rm alpine
alpine

```

### Running a web application

Download/pull the Docker image named `tutum/hell-world`.
```console
vagrant@dockerlab:~$ docker pull tutum/hello-world
Using default tag: latest
latest: Pulling from tutum/hello-world
Image docker.io/tutum/hello-world:latest uses outdated schema1 manifest format. Please upgrade to a schema2 image for better future compatibility. More information at https://docs.docker.com/registry/spec/deprecated-schema-v1/
658bc4dc7069: Pull complete
a3ed95caeb02: Pull complete
af3cc4b92fa1: Pull complete
d0034177ece9: Pull complete
983d35417974: Pull complete
Digest: sha256:0d57def8055178aafb4c7669cbc25ec17f0acdab97cc587f30150802da8f8d85
Status: Downloaded newer image for tutum/hello-world:latest
docker.io/tutum/hello-world:latest
```

Create a detached container for the `tutum/hello-world` image named `helloapp` and port it on port 80.
```console
vagrant@dockerlab:~$ docker run -d -p 80 --name helloapp tutum/hello-world
f7efa01fbbaa949886fb620a0406b3b3c7d8da519c90ec01eb6626b7d34a77fa
```
The container now runs a website and listens on port 80.

Retrieve the IP address of the helloapp container.
```console
vagrant@dockerlab:~$ docker exec -t helloapp /sbin/ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
18: eth0@if19: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
```
The IP address of the container is `172.17.0.2`.

Check whether the website is accessable using the `curl` command and the IP address of the container.
```console
vagrant@dockerlab:~$ curl http://172.17.0.2
<html>
<head>
        <title>Hello world!</title>
        <link href='http://fonts.googleapis.com/css?family=Open+Sans:400,700' rel='stylesheet' type='text/css'>
        <style>
        body {
                background-color: white;
                text-align: center;
                padding: 50px;
                font-family: "Open Sans","Helvetica Neue",Helvetica,Arial,sans-serif;
        }

        #logo {
                margin-bottom: 40px;
        }
        </style>
</head>
<body>
        <img id="logo" src="logo.png" />
        <h1>Hello world!</h1>
        <h3>My hostname is f7efa01fbbaa</h3>    </body>
</html>

```

Next we will display the forwarded port for the hello-app container. This can be done in several ways.
```console
vagrant@dockerlab:~$ docker ps
CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS         PORTS                                                      NAMES
f7efa01fbbaa   tutum/hello-world        "/bin/sh -c 'php-fpm…"   9 minutes ago   Up 9 minutes   0.0.0.0:49153->80/tcp, :::49153->80/tcp                    helloapp
5312746c17e1   portainer/portainer-ce   "/portainer"             6 days ago      Up 2 hours     0.0.0.0:8000->8000/tcp, 0.0.0.0:9000->9000/tcp, 9443/tcp   portainer
vagrant@dockerlab:~$ docker port f7efa01fbbaa
80/tcp -> 0.0.0.0:49153
80/tcp -> :::49153

```
The forwarded port for the helloapp container is port `49153`.

We will check if the website is accessible using this port. This can be by using the `curl` command or opening a browser on your host system and surf to the URL <http:192.168.56.20:PORT>.
```console
curl http://localhost:49153
<html>
<head>
        <title>Hello world!</title>
        <link href='http://fonts.googleapis.com/css?family=Open+Sans:400,700' rel='stylesheet' type='text/css'>
        <style>
        body {
                background-color: white;
                text-align: center;
                padding: 50px;
                font-family: "Open Sans","Helvetica Neue",Helvetica,Arial,sans-serif;
        }

        #logo {
                margin-bottom: 40px;
        }
        </style>
</head>
<body>
        <img id="logo" src="logo.png" />
        <h1>Hello world!</h1>
        <h3>My hostname is f7efa01fbbaa</h3>    </body>
</html>

```

![tutum web](img/lab1/01_hostweb_tutum.PNG)



### Persistent Data
Download/pull the Docker image named `mysql`
```console
vagrant@dockerlab:~$ docker pull mysql
Using default tag: latest
latest: Pulling from library/mysql
07aded7c29c6: Pull complete
f68b8cbd22de: Pull complete
30c1754a28c4: Pull complete
1b7cb4d6fe05: Pull complete
79a41dc56b9a: Pull complete
00a75e3842fb: Pull complete
b36a6919c217: Pull complete
635b0b84d686: Pull complete
6d24c7242d02: Pull complete
5be6c5edf16f: Pull complete
cb35eac1242c: Pull complete
a573d4e1c407: Pull complete
Digest: sha256:4fcf5df6c46c80db19675a5c067e737c1bc8b0e78e94e816a778ae2c6577213d
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest

```

Create a Docker volume named `mysql-data`. This volume will be used to store persistent data within the container.
```console
vagrant@dockerlab:~$ docker volume create mysql-data
mysql-data
```

Display detailed information of the `mysql-data` volume.
```console
vagrant@dockerlab:~$ docker volume inspect mysql-data
[
    {
        "CreatedAt": "2021-10-09T18:13:40Z",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/mysql-data/_data",
        "Name": "mysql-data",
        "Options": {},
        "Scope": "local"
    }
]
```

#### Adding a volume to a container

Start the MySQL container with the following command.
```console
vagrant@dockerlab:~$ docker run --name db -d \
>   -v mysql-data:/var/lib/mysql \
>   -p 3306:3306 \
>   -e MYSQL_DATABASE='appdb' \
>   -e MYSQL_USER='appusr' \
>   -e MYSQL_PASSWORD='letmein' \
>   -e MYSQL_ROOT_PASSWORD='letmein' \
>   mysql:5.7
Unable to find image 'mysql:5.7' locally
5.7: Pulling from library/mysql
07aded7c29c6: Already exists
f68b8cbd22de: Already exists
30c1754a28c4: Already exists
1b7cb4d6fe05: Already exists
79a41dc56b9a: Already exists
00a75e3842fb: Already exists
b36a6919c217: Already exists
5e11fe494f45: Pull complete
9c7de1f889a7: Pull complete
cf6a13d05a76: Pull complete
fc5aa81f393a: Pull complete
Digest: sha256:360c7488c2b5d112804a74cd272d1070d264eef4812d9a9cc6b8ed68c3546189
Status: Downloaded newer image for mysql:5.7
b90101bb8963027f81e32d38234d95c77d87052659f2550e92d57f2492ed04b3
```

Check that you can view the logs of the MySQL container. Do this on a seperate terminal.
```console
vagrant@dockerlab:~$ docker logs -f db
2021-10-09 18:24:11+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.35-1debian10 started.
2021-10-09 18:24:11+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2021-10-09 18:24:11+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.35-1debian10 started.
2021-10-09 18:24:11+00:00 [Note] [Entrypoint]: Initializing database files
2021-10-09T18:24:11.805832Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2021-10-09T18:24:12.016859Z 0 [Warning] InnoDB: New log files created, LSN=45790

[...]
```

Open a MySQL text console inside the container.
```console
vagrant@dockerlab:~$ docker exec -it db mysql -pletmein appdb
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.35 MySQL Community Server (GPL)

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

Create a table named `Catalog` in the `appdb` database and add the records show in the command below.
```console
mysql> CREATE TABLE Catalog(CatalogId INTEGER PRIMARY KEY,Journal VARCHAR(25),Publisher VARCHAR(25),Edition VARCHAR(25),Title VARCHAR(45),Author VARCHAR(25));
Query OK, 0 rows affected (0.01 sec)

mysql> INSERT INTO Catalog VALUES('1','Oracle Magazine','Oracle Publishing','November December 2013','Engineering as a Service','David A. Kelly');
Query OK, 1 row affected (0.02 sec)

mysql>
```

Check if the table has been created correctly. Table names are case sensitive and close your commands with a `;`.
```console
mysql> show tables;
+-----------------+
| Tables_in_appdb |
+-----------------+
| Catalog         |
+-----------------+
1 row in set (0.00 sec)

mysql> SELECT * FROM Catalog;
+-----------+-----------------+-------------------+------------------------+--------------------------+----------------+
| CatalogId | Journal         | Publisher         | Edition                | Title                    | Author         |
+-----------+-----------------+-------------------+------------------------+--------------------------+----------------+
|         1 | Oracle Magazine | Oracle Publishing | November December 2013 | Engineering as a Service | David A. Kelly |
+-----------+-----------------+-------------------+------------------------+--------------------------+----------------+
1 row in set (0.00 sec)

mysql> SELECT CatalogId FROM Catalog;
+-----------+
| CatalogId |
+-----------+
|         1 |
+-----------+
1 row in set (0.00 sec)

mysql>
```

You can access the database in the container if MySQL Workbench is installed on the host system. 

![persistent image 1](img/lab1/02_peristent_mysql_01.PNG)

![persistent image 2](img/lab1/02_persistent_mysql_02.PNG)

![persistent image 3](img/lab1/02_persistent_mysql_03.PNG)

![persistent image 4](img/lab1/02_persistent_mysql_04.PNG)


The database in container can also be accesses by the mysql command line client from within the VM or from the physical system.
```console
vagrant@dockerlab:~$ mysql -h 192.168.56.20 -P 3306 -uroot -pletmein appdb
mysql: [Warning] Using a password on the command line interface can be insecure.
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 11
Server version: 5.7.35 MySQL Community Server (GPL)

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

Executing an SQL query on the database is also possible within the VM or the physical system.
```console
vagrant@dockerlab:~$  mysql -h 192.168.56.20 -P 3306 -uappusr -pletmein appdb <<< "SELECT * FROM Catalog;"
mysql: [Warning] Using a password on the command line interface can be insecure.
CatalogId       Journal Publisher       Edition Title   Author
1       Oracle Magazine Oracle Publishing       November December 2013  Engineering as a Service        David A. Kelly

```

#### Persistence of volumes between container intance

Navigate to the directory to the volume `mysql-data` that has been created earlier. Have a look at the contents. 
```console
vagrant@dockerlab:~$ cd /var/lib/docker/volumes/mysql-data/_data
vagrant@dockerlab:/var/lib/docker/volumes/mysql-data/_data$ ls
appdb     ca-key.pem  client-cert.pem  ib_buffer_pool  ib_logfile0  ibtmp1  performance_schema  public_key.pem   server-key.pem
auto.cnf  ca.pem      client-key.pem   ibdata1         ib_logfile1  mysql   private_key.pem     server-cert.pem  sys
```

Stop the database container and restart it.
```console
vagrant@dockerlab:~$ docker stop db
db
vagrant@dockerlab:~$ docker start db
db
```

Verify that the database is still working and that all data was preserved. 
```console
vagrant@dockerlab:~$ docker exec -it db mysql -pletmein appdb
mysql: [Warning] Using a password on the command line interface can be insecure.
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.35 MySQL Community Server (GPL)

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show tables;
+-----------------+
| Tables_in_appdb |
+-----------------+
| Catalog         |
+-----------------+
1 row in set (0.01 sec)

mysql> select * from Catalog;
+-----------+-----------------+-------------------+------------------------+--------------------------+----------------+
| CatalogId | Journal         | Publisher         | Edition                | Title                    | Author         |
+-----------+-----------------+-------------------+------------------------+--------------------------+----------------+
|         1 | Oracle Magazine | Oracle Publishing | November December 2013 | Engineering as a Service | David A. Kelly |
+-----------+-----------------+-------------------+------------------------+--------------------------+----------------+
1 row in set (0.01 sec)

mysql>
```

Stop and remove the `db` container. 
```console
vagrant@dockerlab:~$ docker stop db
db
vagrant@dockerlab:~$ docker rm db
db
```

Use the `docker run --name db -d \\[...]` command again and verify that the database still exists.
```console
docker run --name db -d \
>   -v mysql-data:/var/lib/mysql \
>   -p 3306:3306 \
>   -e MYSQL_DATABASE='appdb' \
>   -e MYSQL_USER='appusr' \
>   -e MYSQL_PASSWORD='letmein' \
>   -e MYSQL_ROOT_PASSWORD='letmein' \
>   mysql:5.7
66a85fcc8dffc6d6ea86ea333c06b4b9168281fc5964eb171ce027f987456be8

vagrant@dockerlab:~$ mysql -h 192.168.56.20 -P 3306 -uappusr -pletmein appdb <<< "SELECT * FROM Catalog;"
mysql: [Warning] Using a password on the command line interface can be insecure.
CatalogId       Journal Publisher       Edition Title   Author
1       Oracle Magazine Oracle Publishing       November December 2013  Engineering as a Service        David A. Kelly
```
We can access the data even if the container is deleted! This is because the data is being preserved in the volume `mysql-data` we created earlier. 

### Custom images

### Explore the files
Navigate to the directory `/vagrant/labs/static-website`. Take a look at the contents. 
```console
vagrant@dockerlab:/$ cd /vagrant/labs/static-website
vagrant@dockerlab:/vagrant/labs/static-website$ tree
.
├── Dockerfile
└── files
    ├── default.conf
    ├── index.html
    ├── nginx.conf
    ├── site-contents.tar.bz2
    └── works-on-my-machine-ops-problem-now.jpg

1 directory, 6 files
```
This directory contains the files that we will be using to host a static website. 
The file `default.conf` contains settings for the Nginx webserver.
The `site-contents.tar.bz2` file contains the website contents.

Print the contents of the `Dockerfile` file. This contains the instructions for creating an image.
```console
vagrant@dockerlab:/vagrant/labs/static-website$ cat Dockerfile
FROM alpine:latest
LABEL description="This example Dockerfile installs NGINX."
RUN apk add --update nginx && \
    rm -rf /var/cache/apk/* && \
    mkdir -p /tmp/nginx/
COPY files/nginx.conf /etc/nginx/nginx.conf
COPY files/default.conf /etc/nginx/conf.d/default.conf
ADD files/site-contents.tar.bz2 /usr/share/nginx/
EXPOSE 80/tcp
ENTRYPOINT ["nginx"]
CMD ["-g", "daemon off;"]
```

### Building a custom container image

Build the container image (dot in the end denotes the current directory!). 
```console
vagrant@dockerlab:/vagrant/labs/static-website$ docker image build --tag local:static-site .
Sending build context to Docker daemon  417.8kB
Step 1/9 : FROM alpine:latest
 ---> 14119a10abf4
Step 2/9 : LABEL description="This example Dockerfile installs NGINX."
 ---> Running in 504b499be9eb
[...]
Step 9/9 : CMD ["-g", "daemon off;"]
 ---> Running in 730df3a1333c
Removing intermediate container 730df3a1333c
 ---> 62745817c50b
Successfully built 62745817c50b
Successfully tagged local:static-site
```

Show a list of the images in your environment to check if it succeeded.
```console
vagrant@dockerlab:/vagrant/labs/static-website$ docker images
REPOSITORY               TAG           IMAGE ID       CREATED              SIZE
local                    static-site   62745817c50b   About a minute ago   7.24MB
mysql                    5.7           9f35042c6a98   11 days ago          448MB
mysql                    latest        2fe463762680   11 days ago          514MB
hello-world              latest        feb5d9fea6a5   2 weeks ago          13.3kB
portainer/portainer-ce   latest        8377e6877145   2 weeks ago          251MB
alpine                   latest        14119a10abf4   6 weeks ago          5.6MB
tutum/hello-world        latest        31e17b0746e4   5 years ago          17.8MB
```

Start a container based on this image. Forward port 8080 on the host system to port 80 of the container.
```console
vagrant@dockerlab:/vagrant/labs/static-website$ docker run -d -p 8080:80 --name nginx local:static-site
ea2b948286d534064868e807cc063d1a10b452d6dde1dae6eddabfb41647f9c0
```

Check inside the VM if the website is available.
```console
vagrant@dockerlab:/vagrant/labs/static-website$ curl http://localhost:8080/
<html>
    <head>
        <title>Success!</title>
        <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;900&display=swap" rel="stylesheet">
        <style>
        table, th, td {
          border-bottom: 1px solid black;
          padding: 10px;
          border-collapse: collapse;
          text-align: center;
        }
        .center {
          margin-left: auto;
          margin-right: auto;
        }
        h1 {
          text-align: center;
          font-size: 50px;
        }
        p {
          text-align: center;
        }
        .center {
          display: block;
          margin-left: auto;
          margin-right: auto;
          width: 50%;
        }
        * {
          font-family: Montserrat;
          font-size: 20px;
        }
        </style>
    </head>
    <body>
        <h1>Hello world</h1>
        <p>It works.</p>

        <img alt="Works on my machine. OPS problem now."
             src="works-on-my-machine-ops-problem-now.jpg"
             class="center"/>
    </body>
</html>
```

On your host-system, open a browser and surf to the URL <http://192.168.56.20:8080> to see if the website is accessable. 

![custom image website on browser](img/lab1/03_custom_image_01.PNG)

## Layered File system
### Inspecting layers of an image

#### Alpine Image

Display detailed information of the alpine image used in previous part of this report. The information about the filesystem layers is crucial for us.
```console
vagrant@dockerlab:~$ docker inspect alpine:latest
[
    {
        "Id": "sha256:14119a10abf4669e8cdbdff324a9f9605d99697215a0d21c360fe8dfa8471bab",
        "RepoTags": [
            "alpine:latest"
        ],
        "RepoDigests": [
            "alpine@sha256:e1c082e3d3c45cccac829840a25941e679c25d438cc8412c2fa221cf1a824e6a"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2021-08-27T17:19:45.758611523Z",
        "Container": "330289c649db86f5fb1ae5bfef18501012b550adb0638b9193d4a3a4b65a2f9b",
[...]

          },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:e2eb06d8af8218cfec8210147357a68b7e13f7c485b991c288c2d01dc228bb68"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]
```
The image consists of a single layer identified by a SHA-256 checksum:
```console
sha256:e2eb06d8af8218cfec8210147357a68b7e13f7c485b991c288c2d01dc228bb68
```

The previous step can be done in 1 command:
```console
vagrant@dockerlab:~$ docker image inspect alpine:latest | jq ".[]|.RootFS.Layers"
[
  "sha256:e2eb06d8af8218cfec8210147357a68b7e13f7c485b991c288c2d01dc228bb68"
]
```
#### local:static-site image
Display detailed information of the local:static-site image. The filesystem layers are crucial for us.
```console
vagrant@dockerlab:~$ docker inspect local:static-site
[
    {
        "Id": "sha256:62745817c50beb76f440c8a5b71b53816a998085387d9ee3911494bfac61bcda",
        "RepoTags": [
            "local:static-site"
        ],
        "RepoDigests": [],
        "Parent": "sha256:58ebb3861a3473ed0f261c7e83d745a8376597c543190b7efcd3602279543c3f",
        "Comment": "",
        "Created": "2021-10-09T19:36:38.286516658Z",
        "Container": "730df3a1333c812489a45d9e8fc2d583c1f3ac5290a6841cb2ed3a5a5e1a7efa",
[...]

        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:e2eb06d8af8218cfec8210147357a68b7e13f7c485b991c288c2d01dc228bb68",
                "sha256:c9fedb1d5737dc7142f17d57387d3ea550eb1c447e7e94aab21bdb4480e037ba",
                "sha256:4baf61b96b05a235ce9b4ffd8ad5cb4cbeb1d027d98a74fe5dca81f3a843397d",
                "sha256:29a89c36fd1c7cf9d47ef0806ae695637e4ebeef28972e1a5ff7fc40dda04c56",
                "sha256:7281b94000e739df915041a0f2471cdf4363673bbfe1b1312e2659958812da62"
            ]
        },
        "Metadata": {
            "LastTagTime": "2021-10-09T19:36:38.307410358Z"
        }
    }
]
```

The image consists of multiple layers identified by a SHA-256 checksum.
```console
"sha256:e2eb06d8af8218cfec8210147357a68b7e13f7c485b991c288c2d01dc228bb68",
                "sha256:c9fedb1d5737dc7142f17d57387d3ea550eb1c447e7e94aab21bdb4480e037ba",
                "sha256:4baf61b96b05a235ce9b4ffd8ad5cb4cbeb1d027d98a74fe5dca81f3a843397d",
                "sha256:29a89c36fd1c7cf9d47ef0806ae695637e4ebeef28972e1a5ff7fc40dda04c56",
                "sha256:7281b94000e739df915041a0f2471cdf4363673bbfe1b1312e2659958812da62"
```

The previous step can be done in 1 command:
```console
vagrant@dockerlab:~$ docker image inspect local:static-site | jq ".[]|.RootFS.Layers"
[
  "sha256:e2eb06d8af8218cfec8210147357a68b7e13f7c485b991c288c2d01dc228bb68",
  "sha256:c9fedb1d5737dc7142f17d57387d3ea550eb1c447e7e94aab21bdb4480e037ba",
  "sha256:4baf61b96b05a235ce9b4ffd8ad5cb4cbeb1d027d98a74fe5dca81f3a843397d",
  "sha256:29a89c36fd1c7cf9d47ef0806ae695637e4ebeef28972e1a5ff7fc40dda04c56",
  "sha256:7281b94000e739df915041a0f2471cdf4363673bbfe1b1312e2659958812da62"
]
```

Compare the SHA-256 checksum layers of `local:static-site` image with the one of `alpine:latest`.
| local:static-site | alpine:latest |
|-- |--
|e2eb06d8af8218cfec8210147357a68b7e13f7c485b991c288c2d01dc228bb68 | e2eb06d8af8218cfec8210147357a68b7e13f7c485b991c288c2d01dc228bb68 |
|c9fedb1d5737dc7142f17d57387d3ea550eb1c447e7e94aab21bdb4480e037ba | |
|4baf61b96b05a235ce9b4ffd8ad5cb4cbeb1d027d98a74fe5dca81f3a843397d | |
|29a89c36fd1c7cf9d47ef0806ae695637e4ebeef28972e1a5ff7fc40dda04c56 | |
|7281b94000e739df915041a0f2471cdf4363673bbfe1b1312e2659958812da62 | |

The first layer of both containers are identical. This image is resued to build our custom image. 

### Dockerfile directives and layers
Edit the `Dockerfile` for the static-site image and split up the `RUN` directive into 3 seperate lines.
```console
vagrant@dockerlab:/vagrant/labs/static-website$ vim Dockerfile

FROM alpine:latest
LABEL description="This example Dockerfile installs NGINX."
RUN apk add --update nginx
RUN rm -rf /var/cache/apk/*
RUN mkdir -p /tmp/nginx/
COPY files/nginx.conf /etc/nginx/nginx.conf
COPY files/default.conf /etc/nginx/conf.d/default.conf
ADD files/site-contents.tar.bz2 /usr/share/nginx/
EXPOSE 80/tcp
ENTRYPOINT ["nginx"]
CMD ["-g", "daemon off;"]
~
~
:x
```

Rebuild the image with another name (`static-site-2`).
```console
vagrant@dockerlab:/vagrant/labs/static-website$ docker image build --tag local:static-site-2 .
Sending build context to Docker daemon  417.8kB
Step 1/11 : FROM alpine:latest
 ---> 14119a10abf4
Step 2/11 : LABEL description="This example Dockerfile installs NGINX."
 ---> Using cache
 ---> 6d3283e6cb19
[...]
Step 11/11 : CMD ["-g", "daemon off;"]
 ---> Running in 75676753b593
Removing intermediate container 75676753b593
 ---> 4eaa74d01b28
Successfully built 4eaa74d01b28
Successfully tagged local:static-site-2
```

Inspect the layers of the `local:static-site-2` image.
```console
vagrant@dockerlab:/vagrant/labs/static-website$ docker image inspect local:static-site-2 | jq ".[]|.RootFS.Layers"
[
  "sha256:e2eb06d8af8218cfec8210147357a68b7e13f7c485b991c288c2d01dc228bb68",
  "sha256:440f3a684a76de41a31e81ec4ff1df1db72534a388d3f7cc5122058a2031f1ab",
  "sha256:f84020600b81f1b33e18923eb63c358a53dc4eaa3e387e6d1cd68c6c7587341c",
  "sha256:d524c7372d0d47f74bbaf19a5eda5d3456a59175dbda14caec784b38d2cabf6f",
  "sha256:512b3c97f7867d6c1d439db4ad9af448a9a30910a334690636c8fbda53a0c67b",
  "sha256:6ffcc99ca44b1afd00a42557fa8a22406d9101d39ecab47a7e7ddec89c507249",
  "sha256:963aa98be27e9cd50fc2ddeec56cb7cf15a7d843ada7a47bac6f496ca0c3a48a"
]
```
Compare the `local:static-site-2` image layers with the `local:static-site` image from previous.

| local:static-site-2 | local:static-site |
|--|--|
|e2eb06d8af8218cfec8210147357a68b7e13f7c485b991c288c2d01dc228bb68|e2eb06d8af8218cfec8210147357a68b7e13f7c485b991c288c2d01dc228bb68| 
|440f3a684a76de41a31e81ec4ff1df1db72534a388d3f7cc5122058a2031f1ab|c9fedb1d5737dc7142f17d57387d3ea550eb1c447e7e94aab21bdb4480e037ba|
|f84020600b81f1b33e18923eb63c358a53dc4eaa3e387e6d1cd68c6c7587341c|4baf61b96b05a235ce9b4ffd8ad5cb4cbeb1d027d98a74fe5dca81f3a843397d|
|d524c7372d0d47f74bbaf19a5eda5d3456a59175dbda14caec784b38d2cabf6f|29a89c36fd1c7cf9d47ef0806ae695637e4ebeef28972e1a5ff7fc40dda04c56| 
|512b3c97f7867d6c1d439db4ad9af448a9a30910a334690636c8fbda53a0c67b|7281b94000e739df915041a0f2471cdf4363673bbfe1b1312e2659958812da62|
|6ffcc99ca44b1afd00a42557fa8a22406d9101d39ecab47a7e7ddec89c507249| | 
|963aa98be27e9cd50fc2ddeec56cb7cf15a7d843ada7a47bac6f496ca0c3a48a| |

The `local:static-site-2` has 7 layers in total and 2 more layers than the other one. There's only 1 layer identical which is the first one.

### Impact of Dockerfile changes on layers

Make any change to the index.html file. As example add a background 
```console
vagrant@dockerlab:/vagrant/labs/static-website/files$ vim index.html

ADD BACKGROUND COLOR. THE CODE MESSES WITH MARKDOWN FILE THEREFORE
IT'S NOT ADDED HERE;

:x
```

Recreate the `site-contents.tar.bz2` archive with the `index.html` and the jpeg image. 
```console
vagrant@dockerlab:/vagrant/labs/static-website/files$ tar -cjf site-contents.tar.bz2 index.html works-on-my-machine-ops-problem-now.jpg
```

Rebuild the image.
```console
vagrant@dockerlab:/vagrant/labs/static-website$ docker image build --tag local:static-site-3 .
Sending build context to Docker daemon  417.8kB
Step 1/11 : FROM alpine:latest
 ---> 14119a10abf4
Step 2/11 : LABEL description="This example Dockerfile installs NGINX."
 ---> Using cache
 ---> 6d3283e6cb19
[...]
Step 11/11 : CMD ["-g", "daemon off;"]
 ---> Running in 75676753b593
Removing intermediate container 75676753b593
 ---> 4eaa74d01b28
Successfully built 4eaa74d01b28
Successfully tagged local:static-site-3
```

Compare the filesystem layers of the image you built earlier `local:static-site-3` with `local:static-site-2` 

| local:static-site-3 | local:static-site-2 |
| -- | -- |
|e2eb06d8af8218cfec8210147357a68b7e13f7c485b991c288c2d01dc228bb68|e2eb06d8af8218cfec8210147357a68b7e13f7c485b991c288c2d01dc228bb68|
|440f3a684a76de41a31e81ec4ff1df1db72534a388d3f7cc5122058a2031f1ab|440f3a684a76de41a31e81ec4ff1df1db72534a388d3f7cc5122058a2031f1ab|
|f84020600b81f1b33e18923eb63c358a53dc4eaa3e387e6d1cd68c6c7587341c|f84020600b81f1b33e18923eb63c358a53dc4eaa3e387e6d1cd68c6c7587341c"|
|d524c7372d0d47f74bbaf19a5eda5d3456a59175dbda14caec784b38d2cabf6f|d524c7372d0d47f74bbaf19a5eda5d3456a59175dbda14caec784b38d2cabf6f|
|512b3c97f7867d6c1d439db4ad9af448a9a30910a334690636c8fbda53a0c67b|512b3c97f7867d6c1d439db4ad9af448a9a30910a334690636c8fbda53a0c67b|
|6ffcc99ca44b1afd00a42557fa8a22406d9101d39ecab47a7e7ddec89c507249|6ffcc99ca44b1afd00a42557fa8a22406d9101d39ecab47a7e7ddec89c507249|
|99f397849dda62b4d1c82a00c0d019ffd867938ece0c2fcb57c010dc91a2e206|963aa98be27e9cd50fc2ddeec56cb7cf15a7d843ada7a47bac6f496ca0c3a48a|

All the layers except the **last** one are identical. 

## Docker Compose
Go to `/vagrant/labs/todo-app` and and show the contents.
```console
vagrant@dockerlab:$ cd /vagrant/labs/todo-app
vagrant@dockerlab:/vagrant/labs/todo-app$ tree
.
├── docker-compose.yml
├── Dockerfile
├── package.json
├── README.md
├── spec
│   ├── persistence
│   │   └── sqlite.spec.js
│   └── routes
│       ├── addItem.spec.js
│       ├── deleteItem.spec.js
│       ├── getItems.spec.js
│       └── updateItem.spec.js
├── src
│   ├── index.js
│   ├── persistence
│   │   ├── index.js
│   │   ├── mysql.js
│   │   └── sqlite.js
│   ├── routes
│   │   ├── addItem.js
│   │   ├── deleteItem.js
│   │   ├── getItems.js
│   │   └── updateItem.js
│   └── static
│       ├── css
│       │   ├── bootstrap.min.css
│       │   ├── font-awesome
│       │   │   ├── all.min.css
│       │   │   ├── fa-brands-400.eot
│       │   │   ├── fa-brands-400.svg#fontawesome
│       │   │   ├── fa-brands-400.ttf
│       │   │   ├── fa-brands-400.woff
│       │   │   ├── fa-brands-400.woff2
│       │   │   ├── fa-regular-400.eot
│       │   │   ├── fa-regular-400.svg#fontawesome
│       │   │   ├── fa-regular-400.ttf
│       │   │   ├── fa-regular-400.woff
│       │   │   ├── fa-regular-400.woff2
│       │   │   ├── fa-solid-900.eot
│       │   │   ├── fa-solid-900.svg#fontawesome
│       │   │   ├── fa-solid-900.ttf
│       │   │   ├── fa-solid-900.woff
│       │   │   └── fa-solid-900.woff2
│       │   └── styles.css
│       ├── index.html
│       └── js
│           ├── app.js
│           ├── babel.min.js
│           ├── react-bootstrap.js
│           ├── react-dom.production.min.js
│           └── react.production.min.js
└── yarn.lock
```
This directory contains a Node.js demo application and a Dockerfile.

Use this Dockerfile to build an image for the application container `todo-app`.
```console
vagrant@dockerlab:/vagrant/labs/todo-app$ docker image build --tag todo-app .
Sending build context to Docker daemon  4.662MB
Step 1/6 : FROM node:12-alpine
[...]
Step 6/6 : CMD ["node", "src/index.js"]
 ---> Running in f5be3a352939
Removing intermediate container f5be3a352939
 ---> 52e62e85bf4f
Successfully built 52e62e85bf4f
Successfully tagged todo-app:latest
```

The application uses port 3000. Use port forwarding and access the application from the host system.
```console
vagrant@dockerlab:/vagrant/labs/todo-app$ docker run -d -p 3000:3000 --name todo-app-cont todo-app
373e01a116932710d8c93bc9d798019a0039e8bd0f285df118bb1305c923619d
```

![todo app website](img/lab1/04_docker_compose_01.PNG)

Navigate to the `vagrant/labs/todo-app` directory and delete the `node_modules` directory.
```console
vagrant@dockerlab:~$ cd /vagrant/labs/todo-app
vagrant@dockerlab:/vagrant/labs/todo-app$ ls
docker-compose.yml  node_modules  README.md  src             yarn.lock
Dockerfile          package.json  spec       yarn-error.log
vagrant@dockerlab:/vagrant/labs/todo-app$ rm -rf node_modules/
```

Copy the directory `/vagrant/labs/todo-app` to the home directory and go to the copied directory `~/todo-app`.
```console
vagrant@dockerlab:/vagrant/labs/todo-app$ cp -r /vagrant/labs/todo-app ~
vagrant@dockerlab:/vagrant/labs/todo-app$ cd ~/todo-app


```

Open the `docker-compose.yml` file in the directory `dockerlab/labs/todo-app` and examine the content.
```console
vagrant@dockerlab:~/todo-app$ vim docker-compose.yml

version: "3.7"

# In the services: section, list all containers that you need for this
# application stack. In this case, there's two: app and mysql.
services:

  # Container for the Node.js application
  app:
    # Specify the base image's name
    image: node:12-alpine
    # Command needed to build the application container (compare with the
    # Dockerfile!)
    command: sh -c "yarn install && yarn run dev"

    # Port forwarding rules
    ports:
      - 3000:3000

    # Equivalent with Dockerfile WORKDIR directive
    working_dir: /app
    volumes:
      - ./:/app

    # Environment variables needed for the application
    environment:
      # Host name of the database server. This is the name of the MySQL
      # container!
      MYSQL_HOST: db
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos

  # Container for the database backend
  db:
    image: mysql:5.7

    # Named volume for persistent data (volume must be defined! See below)
    volumes:
      - todo-mysql-data:/var/lib/mysql

    # Environment variables
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

# Named volumes used by the containers
volumes:
  todo-mysql-data:
~
:q
```
This file defines two containers: `app` and `db` and instructions to build the image . At last a volume is defined that will be used to make the database contents persistent. 

Stop the running `todo-app` container and run the command shown below.
```console
vagrant@dockerlab:~/todo-app$ docker stop todo-app-cont
todo-app-cont

vagrant@dockerlab:~/todo-app$ docker-compose up -d
Creating network "todo-app_default" with the default driver
Creating volume "todo-app_todo-mysql-data" with default driver
Creating todo-app_db_1  ... done
Creating todo-app_app_1 ... done
vagrant@dockerlab:~/todo-app$
```



List all your containers. Notice that two containers have been made: one for the application and one for the database.
```console
vagrant@dockerlab:~/todo-app$ docker ps
CONTAINER ID   IMAGE                    COMMAND                  CREATED          STATUS          PORTS                                                      NAMES
b9519345a853   mysql:5.7                "docker-entrypoint.s…"   17 minutes ago   Up 17 minutes   3306/tcp, 33060/tcp                                        todo-app_db_1
e2b695e07efc   node:12-alpine           "docker-entrypoint.s…"   17 minutes ago   Up 1 second     0.0.0.0:3000->3000/tcp, :::3000->3000/tcp                  todo-app_app_1

```

Display the IP address of `todo-app_db_1` and `todo-app_app_1`.
```console
vagrant@dockerlab:~/todo-app$ docker inspect todo-app_db_1 | jq ".[]|.NetworkSettings.Networks"
{
  "todo-app_default": {
    "IPAMConfig": null,
    "Links": null,
    "Aliases": [
      "db",
      "b9519345a853"
    ],
    "NetworkID": "1658a60e27141ec5021976528f48237de2fb14d643acc83436d696159660c106",
    "EndpointID": "50b0ea17ccd112824763234fa3f81b047824a3874171ea0cb0f339aca91a4fd3",
    "Gateway": "172.18.0.1",
    "IPAddress": "172.18.0.3",
    "IPPrefixLen": 16,
    "IPv6Gateway": "",
    "GlobalIPv6Address": "",
    "GlobalIPv6PrefixLen": 0,
    "MacAddress": "02:42:ac:12:00:03",
    "DriverOpts": null
  }
}
vagrant@dockerlab:~/todo-app$ docker inspect todo-app_app_1 | jq ".[]|.NetworkSettings.Networks"
{
  "todo-app_default": {
    "IPAMConfig": null,
    "Links": null,
    "Aliases": [
      "app",
      "7c09f531c21f"
    ],
    "NetworkID": "1658a60e27141ec5021976528f48237de2fb14d643acc83436d696159660c106",
    "EndpointID": "1bce1f30237a67efecbb999371c1d3e9f502c8b3a050f40d06fe4b4ee1c36294",
    "Gateway": "172.18.0.1",
    "IPAddress": "172.18.0.2",
    "IPPrefixLen": 16,
    "IPv6Gateway": "",
    "GlobalIPv6Address": "",
    "GlobalIPv6PrefixLen": 0,
    "MacAddress": "02:42:ac:12:00:02",
    "DriverOpts": null
  }
}

```
IP Address of `todo-app_db_1`: 172.18.0.3

IP Address of `todo-app_app_1`: 172.18.0.2

Open a console `/bin/sh` inside the `todo-app_app_1` container.
```console
vagrant@dockerlab:~/todo-app$ docker exec -i -t todo-app_app_1 /bin/sh
/app #

```

Ping and retrieve the IP address of the `db`.
```console
/app # ping -c3 db
PING db (172.18.0.3): 56 data bytes
64 bytes from 172.18.0.3: seq=0 ttl=64 time=0.108 ms
64 bytes from 172.18.0.3: seq=1 ttl=64 time=0.147 ms
64 bytes from 172.18.0.3: seq=2 ttl=64 time=0.096 ms

--- db ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.096/0.117/0.147 ms

/app # getent ahosts db
172.18.0.3      STREAM db
172.18.0.3      DGRAM  db
```
Notice that the IP address of the `db` is `172.18.0.3` same address that we got above.

## Resources

List all sources of useful information that you encountered while completing this assignment: books, manuals, HOWTO's, blog posts, etc.

https://www.whitesourcesoftware.com/free-developer-tools/blog/docker-images-vs-docker-containers/

https://en.wikipedia.org/wiki/Alpine_Linux 

https://www.freecodecamp.org/news/docker-detached-mode-explained/#:~:text=Detached%20mode%2C%20shown%20by%20the,receive%20input%20or%20display%20output.
