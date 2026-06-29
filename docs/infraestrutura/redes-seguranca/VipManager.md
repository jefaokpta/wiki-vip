# VipManager

## Instalação e configuração do VipManager

### 1. Compactar os diretórios necessários para replicação

No servidor com o VipManager atualizado, execute:

```bash
tar czvf jdk-17.tar.gz jdk-17
tar czvf vip-manager.tar.gz vip-manager
```

### 2. Descompactar os arquivos no diretório /opt

```bash
cd /opt
tar zxvf jdk-17.tar.gz
tar zxvf vip-manager.tar.gz
```

### 3. Criar o serviço systemd

Crie o arquivo em /etc/systemd/system/vip-manager.service com o conteúdo abaixo:

```ini
[Unit]
Description=FORNECE DADOS DE RAMAIS E FILAS PARA O VIP, RODA TAMBEM A API ANTI-INAVASION
After=network.target asterisk.service

[Service]
StandardOutput=file:/var/log/vip-manager.log
WorkingDirectory=/opt/vip-manager
ExecStart=/opt/jdk-17/bin/java -jar vip-manager-0.0.1-SNAPSHOT.jar
SuccessExitStatus=143
Nice=10
Environment=ASTERISK_HOST=localhost
Environment=ASTERISK_AMI_USER=vip-events
Environment=ASTERISK_AMI_PASSWORD=vipEventsPass

# ZERAR BLOQUEIOS DIARIAMENTE OU SEMANALMENTE
Environment=ZERAR_BLOQUEIOS=W
#Environment=ZERAR_BLOQUEIOS=D

# SE DEVE USAR O ANTI-INVASION
#Environment=USE_ANTI_INVASION=OFF
Environment=USE_ANTI_INVASION=ON

# ATIVAR INTEGRACAO DIGISAC
Environment=DIGISAC_URI=
Environment=DIGISAC_QUEUE_LIST=

[Install]
WantedBy=multi-user.target
```

### 4. Dar permissão de execução

```bash
chmod +x /etc/systemd/system/vip-manager.service
```

### 5. Habilitar e iniciar o serviço

```bash
systemctl daemon-reload
systemctl enable vip-manager.service
systemctl start vip-manager.service
```

## Retirar um IP do bloqueio

### Acesso pela interface

Acesse o servidor e vá até: Configurações > Bloquear acesso ou "IP"/bloquear-acesso.

### Sem acesso pela interface

Se o IP estiver bloqueado e você não conseguir acessar a interface, faça o seguinte:

1. Remova o IP do arquivo de bloqueios para evitar que seja bloqueado novamente:

```bash
/opt/vip-manager/systemFile/ips-bloqueados.txt
```

2. Remova o IP da lista de bloqueios do IPTABLES.

Verifique se o IP realmente está na lista:

```bash
iptables -nL
```

Para identificar o comando que inseriu o IP:

```bash
iptables -S
```

Exemplo de regra:

```bash
-A ANTI-INVASION -s "IP EXEMPLO"/32 -j DROP
```

Remova a regra com:

```bash
iptables -D ANTI-INVASION -s "IP EXEMPLO"/32 -j DROP
```

> Desta forma, não é necessário reiniciar o VipManager para que os números de filas sejam atualizados.

## Alterações possíveis no comportamento do bloqueio

Se for necessário alterar a limpeza da tabela de IPs bloqueados, edite o parâmetro abaixo no arquivo do serviço:

```ini
Environment=ZERAR_BLOQUEIOS=W
```

Para passar a limpar diariamente, troque para:

```ini
Environment=ZERAR_BLOQUEIOS=D
```

Depois dessa alteração, reinicie o serviço:

```bash
systemctl stop vip-manager.service
systemctl start vip-manager.service
```
