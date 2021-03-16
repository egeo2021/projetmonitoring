# Deploy SWARM

- [Deploy SWARM](#deploy-swarm)
  - [Timezone](#timezone)
    - [Check the timezone](#check-the-timezone)
    - [Set timezone](#set-timezone)
  - [Tuning S.O](#tuning-so)
    - [Check updates](#check-updates)
    - [Install utilities](#install-utilities)
## Timezone

### Check the timezone

```bash
timedatectl status
```

Output:

```bash
               Local time: Mon 2021-01-25 13:45:09 UTC
           Universal time: Mon 2021-01-25 13:45:09 UTC
                 RTC time: Mon 2021-01-25 13:45:10
                Time zone: Etc/UTC (UTC, +0000)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
```

### Set timezone

```bash
timedatectl set-timezone America/Sao_Paulo
```
Verific date
```bash
               Local time: Mon 2021-01-25 10:45:32 -03
           Universal time: Mon 2021-01-25 13:45:32 UTC
                 RTC time: Mon 2021-01-25 13:45:33
                Time zone: America/Sao_Paulo (-03, -0300)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
```


## Tuning S.O

### Check updates

```bash
apt update
apt upgrade -y
```

### Install utilities

```bash
apt install -y net-tools vim nano wget curl tcpdump telnet
```

## Installing Docker

### Installing the docker repository

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

### Grant User to use Docker

```bash
 sudo usermod -aG docker <seu usuario>
```

### Initializing the docker daemon

```bash
systemctl enable --now docker
systemctl status docker
```

### Checking docker version

```bash
docker version
```

### Enable routing in containers

```bash
firewall-cmd --zone=public --add-masquerade --permanent
firewall-cmd --reload
```

### Ativando modo swarm no host
Verific ipaddr
```bash
ip a
2: ens18: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 82:ed:57:56:a9:2c brd ff:ff:ff:ff:ff:ff
    altname enp0s18
    inet 192.168.15.220/24 brd 192.168.15.255 scope global ens18
       valid_lft forever preferred_lft forever
    inet6 fe80::80ed:57ff:fe56:a92c/64 scope link
       valid_lft forever preferred_lft forever
```	   
Enable Swarm
```bash
docker swarm init --advertise-addr 192.168.15.220
```

Saida Terminal
```bash
Swarm initialized: current node (gecgkspeer028ytd9nrxpz9ij) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-2zh65hheq87v8zwnl6ikvppjhiaxynqbeyg7ek71hj0zpe9wmr-ab1703bp4axwyvhxz1978rfav 192.168.15.220:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```


### Inspect all docker networks

```bash
for net in `docker network ls |grep -v NAME | awk '{print $2}'`;do ipam=`docker network inspect $net --format {{.IPAM}}` && echo $net - $ipam; done
```

### Validating network conflict

```bash
ifconfig ens18
```

Output:

```bash
ens18: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.15.220  netmask 255.255.255.0  broadcast 192.168.15.255
        inet6 fe80::80ed:57ff:fe56:a92c  prefixlen 64  scopeid 0x20<link>
        ether 82:ed:57:56:a9:2c  txqueuelen 1000  (Ethernet)
        RX packets 580948  bytes 582103754 (582.1 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 106338  bytes 8076552 (8.0 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
Obs: Nesse exemplo não houve conflict entre as redes...
#### Creating an ingress network with a different range

##### Listing the nodes

```bash
docker node ls
```

##### Changing node availability in swarm

`AT THIS TIME IT WILL NOT START NEW SERVICES`

`<NODE_NAME> IT'S THE NAME OF THE HOSTNAME THAT RETURNS ON THE COMMAND docker node ls`

```bash
docker node update --availability drain <NODE_NAME>
```

##### Removing current network ingress

```bash
$ docker network rm ingress
```

Output:

```bash
WARNING! Before removing the routing-mesh network, make sure all the nodes
in your swarm run the same docker engine version. Otherwise, removal may not
be effective and functionality of newly created ingress networks will be
impaired.
Are you sure you want to continue? [y/N]
```

##### Creating new ingress network

```bash
docker network create \
--driver overlay \
--ingress \
--subnet=192.168.102.0/28 \
--gateway=192.168.102.2 \
--opt com.docker.network.driver.mtu=1200 \
ingress
```

##### Activating node availability again

```bash
docker node update --availability active <NODE_NAME>
```

### Creating a network for our environment

```bash
docker network create --driver overlay net-monitorapp-backend
docker network create --driver overlay net-monitorapp-frontend
docker network create --driver external frontend
```

### Inspect all docker networks again

```bash
for net in `docker network ls |grep -v NAME | awk '{print $2}'`;do ipam=`docker network inspect $net --format {{.IPAM}}` && echo $net - $ipam; done
```

## Adding other manager nodes to the cluster

### Getting Token for manager

```bash
docker swarm join-token manager
```