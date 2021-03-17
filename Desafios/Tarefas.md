# Tarefas

2)

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
	* Usar uma ferramenta para deploy automatizado será um diferencial
	O projeto pode ser automatizado com o Jenkins entregando, entregando o Repositorio por completo e iniciando a stack no servidor remoto. Para este fim, poderá ser preciso fazer algumas modificações, como por exemplo:
	* No zbxproxy.env altera o variavel ZBX_HOSTNAME=ZBXPROXY-{{.Node.Hostname}}, isso setará um hostname no proxy conforme o hostname do Docker Host
    * Há possibilidade tambem de utilzar Variaveis do Jenkins para preenchimento dos das variaveis DB_SERVER_HOST, MYSQL_USER, MYSQL_PASSWORD, MYSQL_DATABASE, ZBX_SERVER_NAME ou qualquer outra da stack, facilitando assim o deploy em diferentes ambientes.

3) Crie uma monitoração para serviço qualquer para ser monitorado por uma instância de zabbix 5.2 
(pode ser a instância criada no exercício 1)
Monitoramento de logs do housekeeping do Zabbix, auxilio para identificar quantidade de registros excluidos a cada housekeeping executado.

Necessario, utilizar dois terminais.

No frontend, acesse Menu > Configuration > Hosts, Clique no host Zabbix server e altere a interface para ser conectado atraves do DNS, em DNS name, altere para "zabbix-agent".

Importe o template em anexo 

5) A equipe de desenvolvimento XPTO deseja monitorar aplicação web de votação, composta de:


7) Monitore os acessos ao webserver do frontend do zabbix (através da página de status do webserver 
(nginx ou apache)).
Criado bind no volume do serviço zabbix-web, para mapeamento do nginx.conf (zbx_data/etc/nginx/zabbix/nginx.conf para /etc/zabbix/nginx.conf dentro do serviço), foi incluido o seguinte conteudo.
```bash
        location = /basic_status {
                stub_status;
        }
}
```
Assim quando é acessado a url http://zabbix-web:8080/basic_status, é apresentado um pequeno status do nginx.
```bash
Active connections: 6 
server accepts handled requests
 9 9 17 
Reading: 0 Writing: 1 Waiting: 5 
```
No frontend do zabbix, crie um novo host, na interface, em DNS name, inclua o nome do serviço do frontend, "zabbix-web", e em Connect to altere para DNS, a conexão será feita por DNS e não por IP. Na aba Template pesquise pelo Template "Nginx by HTTP" e por fim, na aba Macros, Clique no botão "Inherited and host macros", para herdar as macros do template e globais, altere a macro {$NGINX.STUB_STATUS.PORT} para porta 8080, conforme as prints em anexo.
Cadastro do host
![alt tag](/img/tarefa7img1.jpg)
Vincular Template
![alt tag](/img/tarefa7img2.jpg)
Alterar Macros
![alt tag](/img/tarefa7img3.jpg)
Aguarde Coleta
![alt tag](/img/tarefa7img4.jpg)




