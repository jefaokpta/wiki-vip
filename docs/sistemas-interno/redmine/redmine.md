# Instalação e Configuração do Redmine 7.0.0 no HubVIP

## 1. Objetivo

Este documento registra a instalação, configuração e publicação do **Redmine 7.0.0** na infraestrutura do HubVIP.

O Redmine foi instalado como um serviço Docker dentro da stack existente do HubVIP, utilizando:

* Docker Compose
* MySQL 8 existente na infraestrutura
* Nginx como Reverse Proxy
* HTTPS
* Publicação do Redmine em subdiretório (`/redmine`)
* Persistência dos arquivos anexados em volume Docker/host

A URL final de acesso ao sistema é:

```text
https://netuno.vipsolutions.com.br/redmine/
```

---

## 2. Arquitetura

A infraestrutura do HubVIP já utiliza Docker e possui uma rede compartilhada chamada:

```text
hubvip-net
```

O Redmine foi integrado a essa arquitetura sem a necessidade de criar um novo servidor, novo banco MySQL ou novo domínio.

### Arquitetura final

```text
                         INTERNET
                             │
                             │ HTTPS :443
                             ▼
                    ┌──────────────────┐
                    │      NGINX       │
                    │   hubvip-nginx   │
                    └────────┬─────────┘
                             │
             ┌───────────────┼────────────────┐
             │               │                │
             ▼               ▼                ▼
            /              /nodered/        /redmine/
             │               │                │
             ▼               ▼                ▼
         Metabase          Node-RED        Redmine
                                                │
                                                │
                                                ▼
                                         MySQL / redmine
```

O domínio utilizado é:

```text
netuno.vipsolutions.com.br
```

### Serviços publicados

| Caminho     | Serviço  |
| ----------- | -------- |
| `/`         | Metabase |
| `/nodered/` | Node-RED |
| `/redmine/` | Redmine  |

---

## 3. Imagem Docker utilizada

Inicialmente foi utilizada a imagem:

```yaml
image: redmine:6.1
```

Como a instalação ainda não havia sido utilizada em produção, optou-se por começar diretamente com o **Redmine 7**.

Foi adotada a imagem:

```yaml
redmine:7.0.0-trixie
```

A escolha da variante `trixie` foi feita para manter a base Debian da imagem alinhada à versão **Debian 13 (Trixie)** utilizada no servidor Hub.

A versão foi fixada explicitamente para evitar que uma futura atualização automática da tag altere a versão do Redmine sem planejamento.

---

## 4. Serviço Docker do Redmine

O serviço foi configurado no `docker-compose.yml`.

Configuração final:

```yaml
redmine:
  container_name: hubvip-redmine
  build:
    context: ./redmine
    dockerfile: Dockerfile
  image: hubvip-redmine:7.0.0-trixie
  restart: unless-stopped
  depends_on:
    - mysql
  environment:
    REDMINE_DB_MYSQL: mysql
    REDMINE_DB_DATABASE: redmine
    REDMINE_DB_USERNAME: redmine
    REDMINE_DB_PASSWORD: ${REDMINE_DB_PASSWORD}
    REDMINE_RELATIVE_URL_ROOT: /redmine
    RAILS_RELATIVE_URL_ROOT: /redmine
    TZ: America/Sao_Paulo
  volumes:
    - ../volumes/redmine:/usr/src/redmine/files
  networks:
    - hubvip-net
```
## 4.1. Banco de dados

O Redmine utiliza o MySQL já existente na infraestrutura do HubVIP.

O serviço MySQL no Docker Compose possui:

```yaml
mysql:
  container_name: hubvip-mysql
  image: mysql:8
```

O hostname utilizado pelo Redmine é:

```text
mysql
```

Isso ocorre porque `mysql` é o nome do serviço definido no Docker Compose.

O `container_name` do serviço é:

```text
hubvip-mysql
```

Porém, dentro da rede Docker, a referência recomendada é o **nome do serviço**, e não o `container_name`.

Portanto, a configuração:

```yaml
REDMINE_DB_MYSQL: mysql
```

está correta.

---

# 5. Banco e usuário do Redmine

Foi criado previamente no MySQL:

```text
Banco: redmine
Usuário: redmine
Senha: definida através da configuração do ambiente
```

Como o banco e o usuário já estavam configurados corretamente, eles foram preservados durante a reinstalação.

Não foi necessário:

* excluir o usuário;
* recriar o usuário;
* alterar a senha;
* recriar o banco.

---

# 6. Limpeza da instalação anterior

Antes da instalação definitiva do **Redmine 7**, existia uma instalação preliminar do **Redmine 6.1** que ainda não havia sido utilizada.

Como a intenção era iniciar com uma instalação limpa, o container anterior foi removido:

```bash
docker compose stop redmine
docker compose rm -f redmine
```

O banco `redmine` foi mantido, porém suas tabelas foram removidas.

Foi utilizado o seguinte procedimento:

```sql
USE redmine;

SET FOREIGN_KEY_CHECKS = 0;

SET GROUP_CONCAT_MAX_LEN = 1000000;

SELECT GROUP_CONCAT(
    CONCAT('`', table_name, '`')
    SEPARATOR ', '
)
INTO @tables
FROM information_schema.tables
WHERE table_schema = 'redmine'
  AND table_type = 'BASE TABLE';

SET @sql = IF(
    @tables IS NULL,
    'SELECT 1',
    CONCAT('DROP TABLE ', @tables)
);

PREPARE stmt FROM @sql;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;

SET FOREIGN_KEY_CHECKS = 1;
```

Dessa forma:

* o banco `redmine` foi preservado;
* o usuário `redmine` foi preservado;
* a senha foi preservada;
* todas as tabelas da instalação anterior foram removidas;
* a nova instalação pôde inicializar o banco novamente.

> **Importante:** o banco principal do HubVIP (`hub_vip`) não foi alterado.

---
# 7. Persistência dos arquivos

Foi configurado o seguinte volume:

```yaml
volumes:
  - ../volumes/redmine:/usr/src/redmine/files
```

Esse diretório é utilizado pelo Redmine para persistir os arquivos enviados pelos usuários, principalmente anexos.

A estrutura de diretórios utilizada é:

```text
/opt/hub-vip/
├── stack/
│   ├── docker-compose.yml
│   └── redmine/
│       └── Dockerfile
│
└── volumes/
    └── redmine/
```

O banco de dados permanece armazenado no volume próprio do MySQL.

Dessa forma, a persistência dos dados fica organizada da seguinte maneira:

```text
Banco de dados
    ↓
MySQL / volume mysql

Arquivos anexados
    ↓
../volumes/redmine
```

---

# 8. Publicação através do Nginx

O Redmine não foi exposto diretamente à Internet.

O acesso externo é realizado através do **Nginx**, que atua como Reverse Proxy:

```text
Internet
   ↓
HTTPS :443
   ↓
hubvip-nginx
   ↓
hubvip-redmine:3000
```

Foi criado o seguinte bloco de configuração:

```nginx
location /redmine/ {
    proxy_pass http://hubvip-redmine:3000;
    proxy_http_version 1.1;

    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-Host $host;

    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_cache_bypass $http_upgrade;
}
```

O Nginx utilizado possui o seguinte container:

```text
hubvip-nginx
```

---

# 9. Problema encontrado: Redmine publicado em subdiretório

A primeira tentativa de publicação utilizava:

```yaml
REDMINE_RELATIVE_URL_ROOT: /redmine
```

O Redmine carregava corretamente em:

```text
https://netuno.vipsolutions.com.br/redmine/
```

Porém, os links internos eram gerados incorretamente.

Por exemplo, o Redmine gerava URLs como:

```text
https://netuno.vipsolutions.com.br/projects
https://netuno.vipsolutions.com.br/login
https://netuno.vipsolutions.com.br/account/register
```

Quando o comportamento esperado era:

```text
https://netuno.vipsolutions.com.br/redmine/projects
https://netuno.vipsolutions.com.br/redmine/login
https://netuno.vipsolutions.com.br/redmine/account/register
```

Como `/projects`, `/login` e outras rotas não continham o prefixo `/redmine`, essas requisições eram encaminhadas para o `location /` do Nginx.

Consequentemente, acabavam sendo direcionadas ao **Metabase**, que já utiliza a raiz do domínio.

---

# 10. Diagnóstico do problema

Foram realizados diversos testes para identificar a origem do problema e verificar se o prefixo `/redmine` estava sendo corretamente reconhecido pelo Redmine e pelo Rails.

## 10.1. Variável `REDMINE_RELATIVE_URL_ROOT`

Dentro do container, foi executado:

```bash
printenv | grep RELATIVE
```

Foi confirmado que a variável estava definida corretamente:

```text
REDMINE_RELATIVE_URL_ROOT=/redmine
```

Também foi configurada a variável:

```text
RAILS_RELATIVE_URL_ROOT=/redmine
```

Dessa forma, tanto o Redmine quanto o Rails receberam explicitamente o caminho base utilizado na publicação em subdiretório.

---
## 10.2. Configuração do Rails

Foi executado o seguinte comando para verificar o `relative_url_root` reconhecido pelo Rails:

```bash
bundle exec rails runner 'puts Rails.application.config.relative_url_root'
```

O resultado foi:

```text
/redmine
```

Portanto, o Rails reconhecia corretamente o prefixo `/redmine`.

---

## 10.3. Teste das rotas do Rails

Foi executado o seguinte teste:

```bash
bundle exec rails runner 'include Rails.application.routes.url_helpers; puts root_path; puts projects_path'
```

Os resultados foram:

```text
/redmine/
/redmine/projects
```

Isso demonstrou que os helpers de rota do Rails estavam configurados corretamente e geravam as URLs com o prefixo `/redmine`.

---

## 10.4. Teste do HTML efetivamente entregue

Como as rotas internas do Rails estavam corretas, foi necessário verificar o HTML efetivamente entregue ao navegador.

Foi utilizado:

```bash
curl -sk https://netuno.vipsolutions.com.br/redmine/ | grep -oE 'href="[^"]*projects[^"]*"'
```

O resultado foi:

```text
href="/projects"
href="/projects?jump=welcome"
```

Também foi executado:

```bash
curl -sk https://netuno.vipsolutions.com.br/redmine/ | grep -oE 'href="[^"]*(login|account|projects)[^"]*"'
```

Resultado:

```text
href="/projects"
href="/login"
href="/account/register"
href="/projects?jump=welcome"
```

Isso comprovou que os links incorretos estavam sendo efetivamente gerados no HTML entregue pela aplicação ao navegador.

Portanto, o problema não estava sendo causado pelo navegador nem por uma alteração posterior das URLs no cliente.

---

# 11. Configuração do Host Name

Durante o diagnóstico, foi identificado que o Redmine possuía a seguinte configuração padrão:

```ruby
Setting.host_name = 'localhost:3000'
```

Também foi identificado que o protocolo estava configurado para `HTTP`.

Essas configurações foram ajustadas diretamente através do Rails:

```ruby
Setting.host_name = 'netuno.vipsolutions.com.br/redmine'
Setting.protocol = 'https'
```

Após a alteração, foi confirmado:

```text
Setting.host_name
→ netuno.vipsolutions.com.br/redmine

Setting.protocol
→ https
```

Essas configurações são armazenadas no banco de dados do Redmine e, portanto, não exigem a recriação do container.

---

# 12. Diagnóstico definitivo: `config.ru`

Após os testes anteriores, foi identificado que o problema estava relacionado à forma como a aplicação Rack/Puma estava sendo montada.

O arquivo `config.ru` original da imagem Docker era:

```ruby
# This file is used by Rack-based servers to start the application.

require_relative 'config/environment'
run Rails.application
```

Nesse formato, a aplicação Rails era montada diretamente na raiz (`/`).

Para uma implantação em sub-URI, era necessário configurar o Rack para mapear explicitamente:

```text
/redmine
```

para:

```text
Rails.application
```

Essa alteração permite que a aplicação seja executada corretamente dentro do subdiretório utilizado pelo Reverse Proxy.

---

# 13. Dockerfile personalizado

Para que a alteração no `config.ru` não fosse perdida quando o container fosse recriado, não foi realizada uma alteração manual permanente dentro do container.

Foi criado um **Dockerfile personalizado**, baseado na imagem oficial do Redmine:

```dockerfile
FROM redmine:7.0.0-trixie

RUN python3 - <<'PY'
from pathlib import Path

path = Path("/usr/src/redmine/config.ru")

path.write_text("""# This file is used by Rack-based servers to start the application.

require_relative 'config/environment'

map ENV['RAILS_RELATIVE_URL_ROOT'] || '/' do
  run Rails.application
end
""")
PY
```

O novo `config.ru` passou a ser:

```ruby
# This file is used by Rack-based servers to start the application.

require_relative 'config/environment'

map ENV['RAILS_RELATIVE_URL_ROOT'] || '/' do
  run Rails.application
end
```

Dessa forma, a configuração necessária para publicação do Redmine em `/redmine` passa a fazer parte da própria imagem Docker personalizada.

Isso garante que a configuração seja mantida mesmo após operações como:

* recriação do container;
* atualização da stack;
* `docker compose down`;
* `docker compose up`;
* reconstrução da imagem.

---
# 14. Build da imagem personalizada

O serviço passou a utilizar uma imagem Docker personalizada, construída a partir da imagem oficial do Redmine.

A configuração do serviço no `docker-compose.yml` passou a utilizar:

```yaml
build:
  context: ./redmine
  dockerfile: Dockerfile
```

E a imagem resultante foi definida como:

```yaml
image: hubvip-redmine:7.0.0-trixie
```

A imagem foi construída com:

```bash
docker compose build redmine
```

Após a construção, o serviço foi iniciado com:

```bash
docker compose up -d redmine
```

---

# 15. Configuração final do Nginx

Após o ajuste do `config.ru`, foi mantida a seguinte configuração no Nginx:

```nginx
location /redmine/ {
    proxy_pass http://hubvip-redmine:3000;
    proxy_http_version 1.1;

    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-Host $host;

    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_cache_bypass $http_upgrade;
}
```

> **Importante:** a ausência da `/` final no `proxy_pass` é necessária nessa configuração.

Dessa forma, uma requisição como:

```text
/redmine/projects
```

é encaminhada ao container mantendo o prefixo `/redmine`.

O Rack, por sua vez, realiza o mapeamento:

```text
/redmine
    ↓
Rails.application
```

permitindo que o Redmine seja corretamente publicado como uma **sub-URI**.

---

# 16. Validação do Nginx

A configuração do Nginx deve ser validada através do container correto:

```bash
docker exec hubvip-nginx nginx -t
```

Quando a configuração estiver correta, o resultado esperado é:

```text
syntax is ok
test is successful
```

Após alterações na configuração, o reload pode ser realizado com:

```bash
docker exec hubvip-nginx nginx -s reload
```

---

# 17. Resultado final

Após a configuração do `config.ru`, o Redmine passou a funcionar corretamente em:

```text
https://netuno.vipsolutions.com.br/redmine/
```

O problema relacionado aos links internos foi resolvido.

A aplicação passou a gerar e acessar corretamente URLs como:

```text
https://netuno.vipsolutions.com.br/redmine/
https://netuno.vipsolutions.com.br/redmine/projects
https://netuno.vipsolutions.com.br/redmine/login
https://netuno.vipsolutions.com.br/redmine/account/register
```

Também foram carregados corretamente:

* Logotipo;
* CSS;
* JavaScript;
* Imagens;
* Elementos visuais;
* Páginas internas.

Não foi necessário criar um domínio ou registro DNS exclusivo para o Redmine.

---

# 18. Acesso inicial

Como o banco foi inicializado pelo Redmine do zero, foi criado o usuário administrador padrão:

```text
Login: consultar a documentação do projeto
Senha inicial: consultar a documentação do projeto
```

Após o primeiro acesso, a senha deve ser alterada.

Recomenda-se também:

1. Alterar os dados do usuário `admin`;
2. Definir uma senha forte;
3. Criar um usuário administrativo pessoal;
4. Evitar utilizar permanentemente a conta `admin` para atividades rotineiras.

---

# 19. Comandos úteis

## 19.1. Verificar containers

```bash
docker ps
```

## 19.2. Ver logs do Redmine

```bash
docker logs -f hubvip-redmine
```

## 19.3. Verificar variáveis de ambiente

```bash
docker exec hubvip-redmine printenv | grep RELATIVE
```

## 19.4. Verificar configuração do Rails

Entrar no container:

```bash
docker exec -it hubvip-redmine bash
```

Depois executar:

```bash
bundle exec rails runner 'puts Rails.application.config.relative_url_root'
```

## 19.5. Verificar Host Name

```bash
bundle exec rails runner 'puts Setting.host_name'
```

## 19.6. Verificar protocolo

```bash
bundle exec rails runner 'puts Setting.protocol'
```

## 19.7. Verificar configuração do Rack

```bash
docker exec hubvip-redmine cat /usr/src/redmine/config.ru
```

## 19.8. Validar Nginx

```bash
docker exec hubvip-nginx nginx -t
```

## 19.9. Recarregar Nginx

```bash
docker exec hubvip-nginx nginx -s reload
```

## 19.10. Reiniciar Redmine

```bash
docker restart hubvip-redmine
```

## 19.11. Recriar Redmine após alterações no Compose ou Dockerfile

```bash
docker compose build redmine
docker compose up -d redmine
```

---

# 20. Estrutura final resumida

```text
HubVIP
│
├── Docker Compose
│
├── hubvip-nginx
│   └── HTTPS / Reverse Proxy
│
├── hubvip-redmine
│   ├── Redmine 7.0.0
│   ├── Debian Trixie
│   ├── Puma/Rack
│   └── Rails
│
├── hubvip-mysql
│   ├── hub_vip
│   └── redmine
│
└── volumes
    ├── mysql
    └── redmine
        └── files
```

---

# 21. Configurações críticas

As seguintes configurações são essenciais para manter o Redmine funcionando corretamente no subdiretório `/redmine`.

## 21.1. Docker Compose

```yaml
REDMINE_RELATIVE_URL_ROOT: /redmine
RAILS_RELATIVE_URL_ROOT: /redmine
```

## 21.2. Redmine

```ruby
Setting.host_name = 'netuno.vipsolutions.com.br/redmine'
Setting.protocol = 'https'
```

## 21.3. Nginx

```nginx
location /redmine/ {
    proxy_pass http://hubvip-redmine:3000;
}
```

## 21.4. Rack

```ruby
map ENV['RAILS_RELATIVE_URL_ROOT'] || '/' do
  run Rails.application
end
```

A combinação dessas configurações permite que o Redmine seja executado corretamente em:

```text
https://netuno.vipsolutions.com.br/redmine/
```

sem interferir no Metabase publicado na raiz do mesmo domínio.

---

# 22. Considerações para manutenção

A imagem personalizada **não deve ser substituída diretamente** por:

```yaml
image: redmine:7.0.0-trixie
```

sem reaplicar a alteração realizada no `config.ru`.

A imagem atualmente utilizada é construída a partir da imagem oficial do Redmine e contém uma modificação necessária para o funcionamento da aplicação em uma sub-URI.

## Atualização futura do Redmine

Em caso de atualização futura do Redmine, recomenda-se:

1. Verificar a nova versão oficial disponível;
2. Atualizar a instrução `FROM` do Dockerfile;
3. Verificar o `config.ru` da nova versão;
4. Reconstruir a imagem personalizada;
5. Testar o acesso a `/redmine/`;
6. Testar o login;
7. Testar o acesso aos projetos;
8. Testar a criação e edição de issues;
9. Testar o envio e download de anexos;
10. Verificar os logs do Redmine e do Nginx.

Após qualquer alteração na imagem, é importante validar não apenas o carregamento da página inicial, mas também a geração das URLs internas e o funcionamento dos arquivos estáticos.

> **Atenção:** não executar `docker compose down -v` em produção sem avaliar previamente os volumes envolvidos.

O banco `redmine` e o diretório de arquivos anexados são componentes persistentes e devem ser incluídos na estratégia de backup do HubVIP.

---
