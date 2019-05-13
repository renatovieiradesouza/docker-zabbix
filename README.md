# Docker Zabbix

![Zabbix logo] (https://assets.zabbix.com/img/logo/zabbix_logo_500x131.png)

Docker Compose para Zabbix Web rodando em Docker. 

O objetivo desse compose é rodar o zabbix em docker para monitorar APIs com **HTTP AGENT**

Por padrão esse compose não inclui a execução com **HTTPS** e nem o Zabbix Agent, porém veja abaixo como executar com essas opções.

## Executando sem https e sem agent
**Criar as pastas necessárias para armazenamento de volumes do docker**

* mkdir -p /docker/mysql/zabbix/data
* mkdir -p /docker/zabbix/ssl

Rode o docker compose:

```
docker-compose up -d //Faz com que rode em background
docker-compose logs -f //Exibe os logs da sua execução
```
## Executar Zabbix com HTTPS

Inclua mais uma porta no service zabbix-web, deve ficar assim:

```
ports:
     - "80:80"
     - "443:443"
```

## Adicionar Zabbix Agent ao Docker

**Implemente mais um service conforme abaixo:**

```
service:
  zabbix-agent:
  image: zabbix/zabbix-agent:ubuntu-4.2-latest
  restart:
    always
  ports:
    - "10050:10050"
  hostname:NOME_HOST_AQUI
  networks:host
  privileged:true
  volumes:
    - /:/rootfs
    - /var/run:/var/run
  environment:
    - ZBX_HOSTNAME="$(hostname)"
    - ZBX_SERVER_HOST="IP_OU_NOME_DNS_ZABBIX_SERVER"
```
