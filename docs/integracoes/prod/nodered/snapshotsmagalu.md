# Gerenciamento Automático de Snapshots - Magalu Cloud

## Visão Geral

A aba **SNAPSHOTS MAGALU** é responsável por automatizar o ciclo de vida dos snapshots das máquinas virtuais hospedadas na Magalu Cloud.

As rotinas implementadas possuem os seguintes objetivos:

- Criar snapshots diários de todas as instâncias.

- Manter um histórico de backups.

- Excluir automaticamente snapshots antigos.

- Disponibilizar fluxos auxiliares para testes e validações da API.

Todo o processo utiliza exclusivamente a API REST da Magalu Cloud.


# Arquitetura Geral

```
flowchart TD  
  
A\[04:00\<br/\>Criar Snapshots\]  
  
B\[05:00\<br/\>Limpeza\]  
  
A --\> C\[Lista Instâncias\]  
  
C --\> D\[Cria Snapshot\]  
  
B --\> E\[Lista Snapshots\]  
  
E --\> F\[Seleciona antigos\]  
  
F --\> G\[Exclui Snapshot\]
```


# Componentes do Fluxo

| Fluxo | Objetivo |
| - | - |
| Criação de Snapshots | Criar backup diário das VMs |
| Limpeza de Snapshots | Manter apenas os 7 backups mais recentes |
| Testes | Validar chamadas da API |
| Webhook | Endpoint para testes externos |



# Fluxo 1 - Criação Automática de Snapshots

## Objetivo

Criar automaticamente um snapshot para cada instância existente na Magalu Cloud.


## Agendamento

O node **Cron Plus** executa diariamente às:

```
04:00
```

### Expressão Cron

```
0 0 4 \* \* \*
```


## Fluxograma

```
flowchart LR  
  
Cron  
  
--\>  
  
Lista Instâncias  
  
--\>  
  
Seleciona Dados  
  
--\>  
  
Split  
  
--\>  
  
Monta Payload  
  
--\>  
  
POST Snapshot  
  
--\>  
  
Debug
```


## Etapa 1 - Listagem das Instâncias

### Node

```
lista instancias
```

### Requisição

```
GET /compute/v1/instances
```

### Retorno esperado

```
\{  
  "instances": \[  
    \{  
      "id": "...",  
      "name": "Servidor01"  
    \}  
  \]  
\}
```


## Etapa 2 - Filtragem

### Node

```
apenas o necessario
```

Este Function reduz o payload retornado pela API para apenas os campos necessários.

```
\{  
    id,  
    name  
\}
```

Isso reduz o volume de dados trafegados durante o restante do fluxo.


## Etapa 3 - Split

O node **Split** transforma:

```
\[  
    VM1,  
    VM2,  
    VM3  
\]
```

em

```
VM1  
  
↓  
  
VM2  
  
↓  
  
VM3
```

permitindo criar um snapshot individual para cada máquina.


## Etapa 4 - Preparação do POST

### Node

```
prepara POST
```

Monta o corpo da requisição:

```
\{  
    "instance": \{  
        "id": "\<ID DA VM\>"  
    \},  
    "name": "\<NOME DA VM\>-2026-07-13"  
\}
```

O nome do snapshot é composto automaticamente por:

- Nome da VM

- Data atual

Exemplo:

```
hubvip-2026-07-13
```


## Etapa 5 - Criação

### Node

```
cria snapshots
```

Executa:

```
POST /compute/v1/snapshots
```

para cada instância encontrada.


# Fluxo 2 - Limpeza Automática

## Objetivo

Evitar crescimento ilimitado do armazenamento de snapshots.

A rotina mantém apenas:

```
7 snapshots
```

para cada instância.


## Agendamento

Executado diariamente às:

```
05:00
```

### Expressão Cron

```
0 0 5 \* \* \*
```


## Fluxograma

```
flowchart LR  
  
Cron  
  
--\>  
  
Lista Snapshots  
  
--\>  
  
Agrupa por VM  
  
--\>  
  
Ordena  
  
--\>  
  
Mantém 7  
  
--\>  
  
Delete Snapshot
```


## Etapa 1 - Listagem

### Node

```
lista snapshots
```

Executa:

```
GET /compute/v1/snapshots
```


## Etapa 2 - Seleção dos Snapshots

### Node

```
mais q 7 pra deletar
```

Este é o principal algoritmo do fluxo.

### 1. Agrupa

Agrupa todos os snapshots pelo campo:

```
instance.id
```

Exemplo:

```
Servidor A  
  
├── Snapshot 1  
├── Snapshot 2  
├── Snapshot 3  
...
```


### 2. Ordena

Ordena pela data:

```
created\_at
```

do mais novo para o mais antigo.


### 3. Mantém apenas os sete mais recentes

Utiliza:

```
slice(7)
```

Todos os snapshots após a sétima posição entram na lista de exclusão.

### Exemplo

Antes:

```
15 snapshots
```

Depois:

```
Mantém  
  
1  
2  
3  
4  
5  
6  
7  
  
↓  
  
Exclui  
  
8  
9  
10  
11  
12  
13  
14  
15
```


## Etapa 3 - Split

Transforma:

```
\[  
    snapshot1,  
    snapshot2,  
    snapshot3  
\]
```

em mensagens individuais.


## Etapa 4 - Preparação da Exclusão

### Node

```
prepara request
```

Monta dinamicamente:

```
DELETE /compute/v1/snapshots/\{id\}
```


## Etapa 5 - Exclusão

### Node

```
deleta snapshot
```

Executa uma requisição DELETE para cada snapshot selecionado.


# Fluxo de Testes

A aba possui diversos nodes auxiliares destinados exclusivamente a testes.


## Mock Snapshots

### Node

```
mock snapshots
```

Gera uma lista simulada de snapshots.

Seu objetivo é permitir testar toda a lógica de exclusão sem consumir a API da Magalu Cloud.


## Teste de Listagem

Existe um fluxo manual que permite executar:

```
GET /compute/v1/instances
```

e visualizar o retorno.


## Webhook

Endpoint:

```
POST /teste
```

Retorno:

```
HTTP 201
```

Utilizado para validar integrações externas e chamadas HTTP durante o desenvolvimento.


# APIs Utilizadas

## Listar Instâncias

```
GET /compute/v1/instances
```


## Criar Snapshot

```
POST /compute/v1/snapshots
```


## Listar Snapshots

```
GET /compute/v1/snapshots
```


## Excluir Snapshot

```
DELETE /compute/v1/snapshots/\{id\}
```

Todas as chamadas utilizam autenticação através do cabeçalho:

```
x-api-key
```


# Sequência Completa

```
sequenceDiagram  
  
participant Cron  
  
participant NodeRED  
  
participant API  
  
Cron-\>\>NodeRED: 04:00  
  
NodeRED-\>\>API: GET Instances  
  
API--\>\>NodeRED: Lista  
  
loop Cada VM  
  
NodeRED-\>\>API: POST Snapshot  
  
end  
  
Cron-\>\>NodeRED: 05:00  
  
NodeRED-\>\>API: GET Snapshots  
  
API--\>\>NodeRED: Lista  
  
NodeRED-\>\>NodeRED: Agrupa por VM  
  
NodeRED-\>\>NodeRED: Mantém 7  
  
loop Snapshots Antigos  
  
NodeRED-\>\>API: DELETE Snapshot  
  
end
```


# Troubleshooting

| Problema | Possível causa | Verificação |
| - | - | - |
| Snapshot não é criado | API Key inválida | Validar o cabeçalho `x-api-key` |
| Nenhuma instância encontrada | Ambiente sem VMs ou falha na API | Testar `GET /compute/v1/instances` manualmente |
| Snapshot não é excluído | ID inválido ou permissão insuficiente | Conferir a URL montada no node `prepara request` |
| Execução não ocorre no horário esperado | Expressão Cron incorreta ou fuso horário | Validar a configuração do node **Cron Plus** |



# Considerações Finais

O fluxo implementa uma política automática de retenção de backups para as máquinas virtuais hospedadas na Magalu Cloud.

As principais funcionalidades são:

- criação diária de snapshots;

- retenção dos 7 backups mais recentes por instância;

- exclusão automática dos snapshots excedentes;

- utilização de fluxos auxiliares para testes e validação da API;

- automação completa sem necessidade de intervenção manual.

Como melhoria futura, recomenda-se armazenar a **API Key** em uma variável de ambiente ou em um nó de configuração do Node-RED, evitando sua replicação em múltiplos nós HTTP e aumentando a segurança da solução.

