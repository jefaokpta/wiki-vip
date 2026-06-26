# Backup de gravações em servidores compartilhados

## Objetivo

Descrever o procedimento para liberar espaço em um servidor com o volume de gravações cheio, mover os arquivos antigos para outro volume e enviar os dados para o S3.

## 1. Criar um novo volume na AWS

Acesse a AWS e crie um novo volume no servidor que está com o disco de gravações cheio.

1. Entre no console da AWS.
2. Acesse EC2 > Elastic Block Storage > Volumes.
3. Clique em Create Volume.
4. Selecione o Volume type como Cold HDD (sc1).
5. Escolha o tamanho do disco.
6. Selecione a mesma Availability Zone do servidor.
7. Clique em Create volume.

## 2. Anexar o novo volume ao servidor

1. Em EC2 > Elastic Block Storage > Volumes, selecione o volume criado.
2. Clique em Actions > Attach volume.
3. Selecione a instância na qual será feita a movimentação.
4. Defina o device name conforme necessário.
5. Clique em Attach volume.

## 3. Preparar o novo volume no servidor

No servidor, execute os comandos abaixo:

```bash
cat /proc/partitions
fdisk /dev/nvme2n1
# em seguida: n -> enter -> enter -> enter -> enter -> enter -> w -> enter -> q -> enter
mkfs.ext4 /dev/nvme2n1p1
```

Se já houver um volume preparado para essa operação, pode-se iniciar a partir da montagem.

```bash
mount /dev/nvme2n1p1 /mnt
cd /mnt
mkdir NOMEDOSERVIDOR
cd NOMEDOSERVIDOR
nice mv /caminho/das/gravações/gravações_a_serem_movidas .
```

Ao final da movimentação, desmontar o volume:

```bash
umount /dev/nvme2n1p1
```

## 4. Remover o disco do servidor de produção e anexar ao AWSManager

1. Na AWS, selecione o volume com as gravações a serem enviadas para o S3.
2. Clique em Actions > Detach volume.
3. Aguarde até o estado ficar como Available.
4. Clique novamente em Actions e anexe o volume ao servidor AWSManager.

No servidor AWSManager, monte o volume:

```bash
mount /dev/nvme1n1p1 /BKP-SERVIDORES/
```

## 5. Ajustar e executar o script de backup para o S3

Edite o script /root/S3.sh e ajuste as variáveis servidor, ini, fim e caminho conforme a necessidade.

Exemplo:

```bash
servidor="NOMEDOSERVIDOR"
ini="2025-03-01"
fin="2025-03-31"
caminho="/BKP-SERVIDORES/NOMEDOSERVIDOR"
```

Execute o script:

```bash
/root/S3.sh &
```

Para verificar se o processo ainda está em execução:

```bash
ps aux | grep S3
```

## 6. Enviar as gravações para o S3

Edite o script /root/upload_S3.sh e ajuste o nome do servidor e a data do mês.

Exemplo:

```bash
/usr/bin/aws s3 sync /BKP-SERVIDORES/NOMEDOSERVIDOR/NOMEDOSERVIDOR/2025/03/ s3://gravacoes-bkp/NOMEDOSERVIDOR/2025/03/ >> /root/upload_S3.log
```
