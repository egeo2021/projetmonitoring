# 2) Tarefas

* Customizar healthcheck de uma das imagens (ex: frontend ou server ou proxy):

Incluso healthcheck no frontend, verificando a porta 8080, para teste altera a linha abaixo para uma outra porta alem da 8080. 
```bash
     test: ["CMD", "curl", "-f", "http://localhost:8080/"]
```
Recrie a stack, que o serviço do zabbix-web não será iniciado.


* Defina pollers de acordo com o ambiente de testes local
Os pollers podem ser definidos nos envs localizados em:

  | Arquivo          | Servico              |
  | ---------------- | -------------------- |
  | common.env       | Time Zone            |
  | zbxagent.env     | Zabbix Agent         |
  | zbxdbmysql.env   | Mysql                |
  | zbxfrontend.env  | Zabbix Frontend      |
  | zbxserver.env    | Zabbix Server        |
  | zbxproxy.env     | Zabbix Proxy         |
  
* Introduzir alguma nova feature em alguma imagem (ex: externalscripts, drivers odbc..)
Instalado mariadb-connector-odbc.x86_64 na imagem do zabbix-proxy-sqlite3
Cadastre o proxy no Frontend do zabbix para realizar os teste.
	Menu > Administration > Proxies > Create Proxy.
	Name: zabbix-proxy-sqlite3
	Save

* Realizar deploy no swarm/kubernetes local

3) Crie uma monitoração para serviço qualquer para ser monitorado por uma instância de zabbix 5.2 
(pode ser a instância criada no exercício 1)
Monitoramento de logs do housekeeping do Zabbix, auxilio para identificar quantidade de registros excluidos a cada housekeeping executado.

Necessario, utilizar dois terminais.

No frontend, acesse Menu > Configuration > Hosts, Clique no host Zabbix server e altere a interface para ser conectado atraves do DNS, em DNS name, altere para "zabbix-agent".

Importe o template em anexo 





docker service logs --follow monitor_zabbix-server > /tmp/output.log; sleep 1;
