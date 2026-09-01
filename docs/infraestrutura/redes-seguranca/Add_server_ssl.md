# Adicionar servidor no SSL Manager

## Objetivo

Registrar um novo servidor para que ele possa receber certificados automáticos via SSL Manager e recarregar o Apache após a renovação.

## 1. Criar o usuário de deploy

No servidor que será gerenciado, execute:

```bash
adduser deploy-ssl
mkdir /home/deploy-ssl/.ssh
```

## 2. Adicionar a chave pública autorizada

Edite o arquivo de chaves autorizadas:

```bash
vim /home/deploy-ssl/.ssh/authorized_keys
```

Insira a chave pública do servidor SSL Manager, por exemplo:

```bash
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJE2Xl1D3OH6BTiVT185oc1OuMAyP9zh5LhX5Ie3sFf4 deploy-ssl@ssl-manager
```

## 3. Permitir recarga do Apache sem senha

Edite o sudoers:

```bash
vim /etc/sudoers
```

Adicione a seguinte linha:

```bash
deploy-ssl ALL=(root) NOPASSWD: /bin/systemctl reload apache2
```

## 4. Preparar diretório para certificados automáticos

```bash
mkdir /etc/ssl/auto-cert
chown -R deploy-ssl:deploy-ssl /etc/ssl/auto-cert
```

## 5. Registrar o servidor no SSL Manager

No servidor SSL Manager, edite o arquivo `servers.json` e insira o novo servidor.

Depois, execute o pipeline ou a rotina do GitHub Actions responsável por sincronizar a configuração.

## 6. Atualizar a configuração em produção

Quando for colocar o novo certificado em produção é só editar os arquivos `/etc/apache2/ports.conf` e `/etc/apache2/sites-available/default-ssl.conf` e executar os comandos abaixo:

```bash
:1,$s,SSLCertificateKeyFile /etc/ssl/ssl.key/server.key,SSLCertificateKeyFile /etc/ssl/auto-cert/privkey.pem,g
:1,$s,SSLCertificateFile /etc/ssl/ssl.crt/_vipsolutions_com_br.crt,SSLCertificateFile /etc/ssl/auto-cert/fullchain.pem,g
:1,$s,SSLCertificateChainFile /etc/ssl/ssl.crt/_vipsolutions_com_br.ca,SSLCertificateChainFile /etc/ssl/auto-cert/fullchain.pem,g
```

## Observações

- O usuário `deploy-ssl` deve ter acesso apenas ao necessário para automatizar a emissão e a atualização dos certificados.
- A permissão de sudo deve ser restrita ao comando de reload do Apache.
- Sempre valide a configuração após incluir o novo host no SSL Manager.


