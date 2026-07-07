# Certbot e Let's Encrypt

## Objetivo

Documentar o processo básico de instalação, emissão e renovação de certificados TLS com o Certbot e o Let's Encrypt em servidores Linux.

## Pré-requisitos

Antes de iniciar, confirme que:

- o domínio já está apontando para o servidor;
- as portas 80 e 443 estão liberadas no firewall;
- você possui acesso com privilégios de superusuário.
- o serviço web desejado já está instalado e funcionando, como Nginx ou Apache.

## Instalação no Ubuntu/Debian

Atualize os pacotes e instale o Certbot com o plugin do servidor web utilizado:

```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx
```

Para Apache:

```bash
sudo apt install certbot python3-certbot-apache
```

## Emissão do certificado

### Com Nginx

```bash
sudo certbot --nginx -d exemplo.com -d www.exemplo.com
```

O Certbot realiza automaticamente:

- a validação do domínio;
- a configuração do Nginx;
- a instalação do certificado;
- a criação da renovação automática.

### Com Apache

```bash
sudo certbot --apache -d exemplo.com -d www.exemplo.com
```

## Renovação automática

Os certificados emitidos pelo Let's Encrypt têm validade curta e precisam ser renovados periodicamente. O Certbot normalmente configura isso automaticamente via cron ou systemd.

Para validar a renovação sem aplicar alterações:

```bash
sudo certbot renew --dry-run
```

Se quiser renovar manualmente:

```bash
sudo certbot renew
```

## Verificação dos arquivos do certificado

Os certificados ficam normalmente em diretórios como:

```bash
/etc/letsencrypt/live/exemplo.com/
```

## Observações

- Em ambientes com firewall, proxy ou balanceador, é necessário garantir que as validações do Let's Encrypt sejam acessíveis pela internet.
- Caso a validação falhe, verifique os logs do Certbot e a configuração do servidor web.
- Para domínios com múltiplos hosts, repita o comando com todos os nomes desejados.
