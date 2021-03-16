# Tarefas

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

* Realizar deploy no swarm/kubernetes local