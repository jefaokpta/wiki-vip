# MySQL

Guia de instalação e configuração do MySQL para uso na plataforma VIP 2.

## Pré-requisitos

- Distribuição Linux compatível
- `libaio1` e `libaio-dev` instalados
- Permissão de administrador para criar usuários e serviços

## Instalação do MySQL banco de dados

1. Crie o grupo e o usuário do MySQL:

```sh
groupadd mysql
useradd -r -g mysql mysql
```

2. Baixe e extraia a distribuição binária do MySQL em `/usr/local/mysql`.

3. Ajuste as permissões do diretório:

```sh
cd /usr/local/mysql
chown -R mysql .
chgrp -R mysql .
```

4. Inicialize o banco de dados:

```sh
scripts/mysql_install_db --user=mysql
```

5. Ajuste as permissões finais:

```sh
chown -R root .
chown -R mysql data
```

6. Opcional: copie a configuração padrão:

```sh
cp support-files/my-medium.cnf /etc/my.cnf
```

7. Inicie o servidor MySQL:

```sh
bin/mysqld_safe --user=mysql &
```

## Configuração do usuário root

Após iniciar o servidor, conecte-se ao MySQL e configure a senha root:

```sql
mysql> use mysql;
mysql> update user set password=PASSWORD('NEASSWORD') where User='root';
mysql> flush privileges;
mysql> grant all privileges on *.* to 'root'@'localhost';
```

## Inicialização automática

Opcionalmente, instale o script de inicialização:

```sh
cp support-files/mysql.server /etc/init.d/mysql.server
```

## Observações

- Se a instalação for feita antes de um upgrade de sistema, garanta que as dependências sejam satisfeitas.
- Ajuste os caminhos e configurações conforme a estrutura de diretórios do servidor.
