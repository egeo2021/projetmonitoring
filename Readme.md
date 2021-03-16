# Desafio Docker swarm - Stack funcionando com Zabbix e Mysql 8.0

## Contents

- Zabbix:
  - Zabbix Server: zabbix-server:10051
  - Zabbix Agent: zabbix-agent:10050
  - Zabbix Frontend: http://zabbix-frontend:8880
- Database:
  - Mysql naõ está em container, e sim instalado local na mesma maquina: Neste exmeplo utilizando o IP 192.168.15.25
	
## Requirements
Esse laboratorio foi utilizado um Debian 10

- 1 VM com Docker HOST e Mysql instalado.
- docker - 192.168.15.25
- database - 192.168.15.25

 
## Step by step to deploy Zabbix in Docker Swarm

- [Deploy MySQL 8](steps/1_deploy_db_mysql8_debian.md)
- [Deploy Docker Swarm](steps/2_deploy_swarm_debian.md)
- [Deploy Stack Zabbix](steps/4_deploy_stack_monitor.md)

## Como utilizar:

[Install Prerequisites](steps/2_deploy_swarm_debian.md)  

- Copia o projeto para a estação, juntamente com os submodulos:
  ```sh
  $ git clone https://github.com/egeo2021/projetmonitoring.git
  $ cd deploy-monitor-swarm

  ```
- **Se necessario** Edite as configurações de timezone, que é utilizado em todo o amebinte:

  ```sh
  $ vim envs/common.env
  ```

  | Environment      | Default              |
  | ---------------- | -------------------- |
  | TZ               | America/Sao_Paulo    |
  | PHP_TZ           | America/Sao_Paulo    |

  
- **Necessario** editar as configurações de conexões ao banco de dados:

  ```sh
  $ vim envs/zbxdbmysql.env
  ```

  | Environment      | Default              |
  | ---------------- | -------------------- |
  | DB_SERVER_HOST   | 192.168.15.25        |
  | DB_SERVER_PORT   | 3306                 |
  | MYSQL_USER       | zabbix               |
  | MYSQL_PASSWORD   | ZaBBix2021!          |
  | MYSQL_DATABASE   | zabbix               |

- Options for Zabbix Frontend
  ```sh
  $ vim envs/zbxfrontend.env
  ```
 
- Options for Zabbix Server
  ```sh
  $ vim envs/zbxserver.env
  ```

- Options for Zabbix Agent
  ```sh
  $ vim envs/zbxagent.env
  ```

## Deploy Stack Monitor

[Deploy Stack Monitor](steps/4_deploy_stack_monitor.md)
  ```sh  
  $ docker stack deploy -c docker-compose.yml monitor
  ```
