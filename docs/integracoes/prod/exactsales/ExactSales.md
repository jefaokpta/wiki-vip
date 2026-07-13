# Integração Exact Sales

## Visão Geral

Este documento descreve o processo de integração entre o **Exact Sales** e o **VIP**, incluindo:

- obtenção do Token de API;

- identificação do UserID;

- configuração da integração no VIP;

- configuração dos usuários;

- habilitação do clique para discagem (Click-to-Call);

- funcionamento das gravações.


# Pré-requisitos

Antes de iniciar, tenha em mãos:

- acesso administrativo ao Exact Sales;

- acesso administrativo ao VIP;

- OrgID fornecido pela Exact;

- Collection Postman (opcional) para testes.


# 1. Obtendo o Token da API

No Exact Sales:

**Configurações → Integrações**

Copie o **Token** da API.

> Este token será utilizado em todas as chamadas para a API do Exact.

> 📷 *Inserir imagem da tela de Integrações*


# 2. Obtendo o UserID

Utilize o Token obtido anteriormente na requisição:

```
  
Listar SDRs
```

Informe o Token no Header da requisição.

O retorno será semelhante a:

```
\{  
    "id": 12345,  
    "name": "Usuário"  
\}
```

O campo **id** corresponde ao **UserID**.

> Caso existam vários vendedores, cada um possuirá um ID diferente.

> 📷 *Inserir imagem da resposta da API*


# 3. Configuração no VIP

No cadastro da integração informe:

| Campo | Valor |
| - | - |
| Token | Token da API |
| UserID | ID obtido anteriormente |
| OrgID | Informado pela Exact |
| URL | [https://api.exactspotter.com/api/v2/call](https://api.exactspotter.com/api/v2/call) |
| Bina | Opcional |


### Observações

- O OrgID precisa ser solicitado para a Exact.

- A URL sempre será:

```
  
https://api.exactspotter.com/api/v2/call
```

- A Bina somente deve ser preenchida quando houver necessidade de utilizar uma DDR específica por vendedor.

> 📷 *Inserir imagem da configuração do VIP*


# 4. Configuração do Usuário

No Exact:

**Equipe → Editar usuário**

Configure o campo **Telefone 1**:

- se o vendedor utiliza fila de atendimento, informe o número do Agente;

- caso contrário, informe o Ramal.

> 📷 *Inserir imagem*


# 5. Configuração do Click-to-Call

Acesse:

**Configurações → Gerenciar Configurações Avançadas → Outros**

No campo:

**Customização do Link de Telefone**

Informe:

```
  
tel:\{tel\}
```

Isso permitirá que o clique sobre o telefone do cliente abra automaticamente o softphone.

> 📷 *Inserir imagem*


# Funcionamento da Gravação

Após realizar uma ligação:

- a gravação ficará anexada ao card do cliente;

- o histórico poderá ser consultado diretamente pelo usuário.


# Limitações

> Esta integração suporta apenas **Softphone**.

Para utilização com **Webphone**, é necessário contratar o serviço diretamente com a Exact.


# Fluxo da Integração

```
flowchart TD  
  
A\[Obter Token\] --\> B\[Listar SDRs\]  
  
B --\> C\[Obter UserID\]  
  
C --\> D\[Solicitar OrgID\]  
  
D --\> E\[Configurar VIP\]  
  
E --\> F\[Configurar Usuário\]  
  
F --\> G\[Configurar Click-to-Call\]  
  
G --\> H\[Ligações com gravação\]
```

