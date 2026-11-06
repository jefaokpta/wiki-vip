# Novo Servidor MGL

## Procedimento para criação do servidor

1. Entrar em `Virtual Machine` > `Snapshots`.
2. Localizar o snapshot mais recente do `IPBCenter` e clicar nas três bolinhas.
3. Selecionar `Restaurar`.
4. Escolher a zona de localização (normalmente final `A`).
5. Clicar em `Continuar`.
6. Selecionar o tipo de instância: `(BV2-4-40)`.
7. Clicar em `Continuar`.
8. Selecionar a chave para acesso ao servidor.
9. Clicar em `Continuar`.
10. Inserir o nome do novo servidor.
11. Clicar em `Ir para revisão`.
12. Aguardar o servidor ficar pronto para uso.

Enquanto a VM é provisionada, já é possível fazer as liberações necessárias:

1. Clicar no nome do servidor e depois em `Rede`.
2. Clicar em `Adicionar grupo de segurança`.
3. Selecionar o grupo `padrao_vip`.

## Criar volume de gravações

1. Acessar `Block Storage`.
2. Clicar em `Criar volume`.
3. Selecionar a zona em que a VM foi criada.
4. Definir a quantidade mínima de IOPS (não é necessário alto desempenho, pois o disco será apenas de armazenamento de gravações).
5. Definir o tamanho em `1TB`.
6. Nomear o disco como `gravacao_NOME-DO-SERVIDOR`.
7. Clicar em `Criar volume`.
8. Clicar nos três pontinhos do disco criado e selecionar `Anexar instância`.
9. Selecionar o servidor e confirmar com `OK`.

## Preparar o disco no servidor

Quando o servidor estiver pronto, acesse-o via SSH.

Formatar o disco de 1 TB inserido. Em geral, a identificação do disco no servidor é `/dev/vdb` (pode ser confirmada com o comando abaixo):

```bash
cat /proc/partitions
```

Executar:

```bash
fdisk /dev/vdb
```

No `fdisk`, digitar:

```bash
n
# pressionar Enter 4 vezes
w
```

Em seguida, formatar a partição:

```bash
mkfs.ext4 /dev/vdb1
```

Verificar se em `/etc/rc.local` existe a linha que monta o disco no diretório de gravações do VIP. A linha deve estar descomentada:

```bash
mount /dev/vdb1 /var/www/vip/app/asterisk/records/
```

Reinicie a VM para validar se a montagem automática ocorrerá corretamente.

## Banco de dados

Há duas opções:

- `Opção 1`: limpar o banco deste servidor.
- `Opção 2`: pedir para o Salomão limpar o banco do servidor e disponibilizar a estrutura e os dados básicos mais atualizados.

### Opção 2: restaurar banco limpo

Acesse o MySQL:

```bash
mysql -uroot -p
```

Recriar a base `vip`:

```sql
DROP DATABASE vip;
CREATE DATABASE vip;
```

Copiar o arquivo do banco para o servidor, por exemplo em `/root/BACKUP`, e restaurar:

```bash
mysql -uroot -p vip < /root/BACKUP/banco_vazio.sql
```

### Opção 1: continuar com o banco existente

Acesse o arquivo `/etc/asterisk/sip.conf` e altere o IP em `externaddr` para o IP externo do servidor.

Depois, recarregue o Asterisk:

```bash
asterisk -rx "sip reload"
```

## Testes de validação

Antes de finalizar a implantação, validar os seguintes itens:

- Testar ramal.
- Testar ligação de saída.
- Testar acesso à fila:
  - login
  - pausa
  - despausa
  - logoff
- Validar supervisão:
  - pausa
  - despausa
  - logoff do agente
- Verificar se a ligação gerou relatório e gravação.
- No dia seguinte, confirmar se a gravação continua disponível no relatório e se o script de conversão criou a pasta do dia anterior no diretório de gravações.

## Ajustes de scripts e monitoramento

Acessar o cron e executar:

```bash
:1,$s,IPBCenter,SERVIDOR,g
```

Em seguida, editar os arquivos abaixo:

- `/etc/SCRIPTS/Backup/funcoes/Variaveis.inc`
  - alterar `nome_cliente=`
- `/etc/SCRIPTS/Watchdogs/Watchdog.Asterisk.IPBCenter.sh`
  - alterar o texto `IPBCenter` para o nome do cliente/servidor
  - pode-se executar novamente o comando do `vim` no cron ou editar diretamente o arquivo
- Renomear o arquivo:

```bash
mv /etc/SCRIPTS/Watchdogs/Watchdog.Asterisk.IPBCenter.sh /etc/SCRIPTS/Watchdogs/Watchdog.Asterisk.SERVIDOR.sh
```

Nos arquivos dentro de `/etc/SCRIPTS/Gerenciamentos`:

- `IPBCenter.Verifica.Espaco.Critico.sh` -> `SERVIDOR.Verifica.Espaco.Critico.sh`
- `IPBCenter.Verifica.Espaco.Relatorio.sh` -> `SERVIDOR.Verifica.Espaco.Relatorio.sh`

Dentro desses arquivos, ajustar o conteúdo de `cliente=` para o nome do cliente/servidor.

Em `/etc/SCRIPTS/Licenciados/Contador.sh`:

- alterar a linha `server=` para o nome do cliente/servidor
- se houver quantidade de canais para alerta deste servidor, informar em `max=`

## SSL Manager / DNS

Inserir o servidor em `servers.json` do `ssl-manager`.

Criar os DNSs:

- `admin-SERVIDOR`
- `login-SERVIDOR`
- `sip.SERVIDOR`

## Observação

> O processo pode variar de acordo com a estrutura do ambiente e com a necessidade de limpeza do banco ou ajustes específicos do cliente/servidor.
