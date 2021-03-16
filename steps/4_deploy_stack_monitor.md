# Deploy Zabbix Server, Frontend e Grafana no Docker

- [Deploy Zabbix Server, Frontend no Docker]
  - [Deploy stack Zabbix](#deploy-stack-zabbix)
    - [Install GIT](#install-git)
    - [Cloning repo](#clonando-depositório)
    - [Deploy stack](#create-a-stack)
    - [List stacks](#listando-stacks-disponiveis)
    - [Status services](#listando-status-dos-serviços)
    - [Verific logs](#verificar-logs)
    - [Remove stack](#removendo-stack)


## Deploy stack Zabbix

### Install GIT

```bash
apt install -y git
```

### Cloning repo

```bash
cd /mnt/data-docker
git clone https://github.com/egeo2021/projetmonitoring.git
```

### Deploy stack

```bash
cd deploy-monitor-swarm
docker stack deploy -c docker-compose.yml monitor
```

### List stacks

```bash
docker stack ls
```

### Status services

```bash
docker service ls
```

### Verific logs

```bash
docker service logs -f monitor_zabbix-server
docker service logs -f monitor_zabbix-web
```
 
## Verific erros
```bash
docker service ps --no-trunc monitor_zabbix-server
docker service ps --no-trunc monitor_zabbix-web
```
### Remove stack

```bash
docker stack rm monitor
```