# Manual Técnico de Integração: Recebimento em Tempo Real (Gmail API + Pub/Sub + Node-RED)

## Objetivo

Configurar o recebimento de e-mails em tempo real (Push Notifications) da caixa postal do Gmail para o Node-RED, permitindo o processamento automático de respostas, para validar o uso dessa API em outras aplicações da Vip.

## Pré-Requisitos

Antes de iniciar a configuração, é necessário possuir:

- Acesso ao **Google Cloud Platform (GCP)** com permissões para criação de recursos.
- Acesso de administrador ao ambiente do **Node-RED**.
- Uma URL pública HTTPS apontando para o Node-RED, podendo ser:
  - Domínio próprio com certificado SSL.
  - NGROK configurado para encaminhamento HTTPS.

---

# Parte 1 — Conceito da Arquitetura

## Visão Geral

Ao invés de o Node-RED consultar periodicamente a caixa postal do Gmail (processo conhecido como **Polling**), esta integração utiliza uma arquitetura baseada em eventos (**Event-Driven**).

Neste modelo, o próprio Google notifica imediatamente quando um novo e-mail é recebido, reduzindo consumo de recursos, diminuindo latência e eliminando consultas desnecessárias à API.

### Fluxo da Arquitetura

1. Um novo e-mail chega à caixa postal **automacao@vipsolutions.com.br**.

2. O Gmail gera automaticamente um evento de notificação.

3. Essa notificação é enviada ao **Google Cloud Pub/Sub**, que atua como barramento de mensagens.

4. O **Google Cloud Pub/Sub** realiza uma requisição **HTTP POST (Webhook)** para a URL pública do Node-RED.

5. O Node-RED recebe a notificação, identifica a nova mensagem, consulta os detalhes através da **Gmail API** e inicia o processamento automático do ticket.

---

## Fluxo Resumido

```text
Novo E-mail
      │
      ▼
 Gmail API
      │
      ▼
Google Cloud Pub/Sub
      │
      ▼
Webhook HTTPS
      │
      ▼
Node-RED
      │
      ▼
Consulta Gmail API
      │
      ▼
Processamento do Ticket
```

## Vantagens desta Arquitetura

- Comunicação em tempo real.
- Elimina consultas periódicas (Polling).
- Menor consumo de requisições da API Gmail.
- Menor utilização de CPU e processamento no servidor.
- Escalabilidade para grande volume de mensagens.
- Arquitetura orientada a eventos (Event-Driven), recomendada pelo Google.

# Parte 2 — Configuração no Google Cloud Platform (GCP)

Esta etapa consiste em configurar a infraestrutura necessária no Google Cloud Platform para que o Gmail possa enviar notificações em tempo real ao Node-RED utilizando o serviço **Google Cloud Pub/Sub**.

---

# Etapa 2.1 — Ativar a API Cloud Pub/Sub

Antes de criar os recursos, é necessário habilitar a API do Pub/Sub no projeto do Google Cloud.

## Passo a Passo

1. Acesse o **Google Cloud Console**.

2. Selecione o projeto da **VIP Solutions** utilizado anteriormente para configurar a Conta de Serviço.

3. No campo de pesquisa superior, procure por:

   ```
   Cloud Pub/Sub API
   ```

4. Clique no resultado correspondente.

5. Caso a API ainda não esteja habilitada, clique em **Ativar (Enable)**.

---

# Etapa 2.2 — Criar o Tópico (Topic)

O **Topic** é o canal onde o Gmail publicará os eventos de novos e-mails.

## Passo a Passo

1. No menu lateral esquerdo, acesse:

   ```
   Pub/Sub
       └── Topics
   ```

2. Clique em **Create Topic**.

3. No campo **Topic ID**, informe exatamente:

   ```text
   gmail-incoming-emails
   ```

4. Mantenha todas as demais configurações no padrão.

5. Clique em **Create**.

---

## Nome Completo do Tópico

Após a criação, o Google gera automaticamente o identificador completo do recurso.

O formato será semelhante ao exemplo abaixo:

```text
projects/SEU_PROJECT_ID/topics/gmail-incoming-emails
```

> **Importante**
>
> Guarde este identificador completo. Ele será utilizado posteriormente durante a configuração da Gmail API.

---

# Etapa 2.3 — Conceder Permissão para o Gmail Publicar no Tópico

Por padrão, apenas usuários autorizados podem publicar mensagens em um Topic.

Como será o próprio serviço do Gmail quem enviará as notificações, é necessário conceder permissão à conta de serviço oficial utilizada pelo Google para Push Notifications.

## Conta de Serviço do Gmail

```text
gmail-api-push@system.gserviceaccount.com
```

---

## Passo a Passo

1. Abra o tópico **gmail-incoming-emails**.

2. No painel lateral direito, clique na aba **Permissions**.

3. Clique em **Add Principal**.

4. No campo **New Principals**, informe:

   ```text
   gmail-api-push@system.gserviceaccount.com
   ```

5. No campo **Role**, selecione:

   ```text
   Pub/Sub
       └── Pub/Sub Publisher
   ```

6. Clique em **Save**.

---

## Resultado Esperado

Ao finalizar esta etapa, o serviço do Gmail estará autorizado a publicar notificações de novos e-mails no tópico criado, permitindo que o Google Cloud Pub/Sub encaminhe esses eventos para o Node-RED nas próximas etapas da configuração.

# Etapa 2.4 — Criar a Assinatura Push (Subscription)

A **Subscription** é o componente responsável por receber as mensagens publicadas no **Topic** e encaminhá-las automaticamente para o Node-RED através de um **Webhook HTTPS**.

Sem a assinatura, as mensagens permanecerão armazenadas no tópico e nunca chegarão ao Node-RED.

---

## Passo a Passo

1. No menu lateral do **Google Cloud Pub/Sub**, acesse:

   ```text
   Pub/Sub
       └── Subscriptions
   ```

2. Clique em **Create Subscription**.

3. Preencha os campos conforme abaixo.

### ID da Assinatura

```text
sub-gmail-nodered
```

### Topic

Selecione o tópico criado anteriormente.

Exemplo:

```text
projects/SEU_PROJECT_ID/topics/gmail-incoming-emails
```

### Delivery Type

Selecione:

```text
Push
```

### Push Endpoint URL

Informe a URL pública do endpoint criado no Node-RED.

Exemplo:

```text
https://sua-url-publica.com/webhook/gmail
```

> **Importante**
>
> O endpoint deve estar acessível pela Internet utilizando **HTTPS**. Caso esteja em ambiente de desenvolvimento, é possível utilizar ferramentas como **NGROK** para expor temporariamente o Node-RED.

### Ack Deadline

Defina o prazo de confirmação (**Ack Deadline**) para:

```text
30 segundos
```

4. Após revisar as configurações, clique em **Create**.

---

## Resultado Esperado

Ao finalizar esta etapa, toda nova mensagem publicada pelo Gmail no tópico será enviada automaticamente para o endpoint HTTP configurado no Node-RED.

---

# Parte 3 — Construção do Fluxo no Node-RED

Nesta etapa será desenvolvido o fluxo responsável por receber as notificações enviadas pelo Google Cloud Pub/Sub, consultar a Gmail API para obter o conteúdo completo da mensagem e iniciar o processamento do ticket.

O fluxo será dividido em três fases:

- Receber a notificação do Pub/Sub.
- Buscar o e-mail completo utilizando a Gmail API.
- Processar a mensagem recebida.

---

## Estrutura do Fluxo

```text
HTTP In
    │
    ▼
Extrai HistoryId
    │
    ▼
Gerar Token OAuth
    │
    ▼
HTTP Request (Get Message)
    │
    ▼
HTTP Response
```

---

# Etapa 3.1 — Criar o Endpoint do Webhook

O primeiro componente do fluxo será um endpoint HTTP responsável por receber as notificações enviadas pelo Google Cloud Pub/Sub.

Sempre que um novo e-mail chegar à caixa postal monitorada, o Pub/Sub realizará uma requisição **HTTP POST** para este endpoint.

## Configuração do nó HTTP In

Adicione um nó **HTTP In** ao fluxo e configure-o conforme abaixo.

| Propriedade | Valor |
|------------|-------|
| Method | POST |
| URL | `/webhook/gmail` |

---

## Resposta ao Google Cloud Pub/Sub

Logo após o **HTTP In**, conecte um nó **HTTP Response**.

Este nó deve responder imediatamente com o código HTTP:

```text
200 OK
```

Responder rapidamente é importante porque o Google Cloud Pub/Sub considera a mensagem entregue somente quando recebe uma resposta de sucesso.

Caso o endpoint não responda dentro do prazo ou retorne erro, o Pub/Sub entenderá que houve falha na entrega e tentará reenviar a mesma notificação diversas vezes.

# Etapa 3.2 — Decodificar a Notificação do Pub/Sub

Quando o Google Cloud Pub/Sub envia uma notificação ao Node-RED, os dados do evento não chegam diretamente em formato JSON.

O conteúdo da mensagem é enviado codificado em **Base64**, sendo necessário decodificá-lo antes do processamento.

A estrutura recebida possui o seguinte formato:

```json
{
  "message": {
    "data": "BASE64..."
  }
}
```

Após a decodificação, o conteúdo será semelhante a:

```json
{
  "emailAddress": "automacao@vipsolutions.com.br",
  "historyId": "123456789"
}
```

Onde:

- **emailAddress** → Caixa postal monitorada.
- **historyId** → Identificador que informa que houve alteração na caixa de e-mails.

---

## Criando o nó "Decodificar PubSub"

Adicione um nó do tipo **Function** logo após o **HTTP In**.

Nome sugerido:

```text
Decodificar PubSub
```

Utilize o seguinte código JavaScript:

```javascript
// O Pub/Sub entrega o dado dentro de msg.payload.message.data (Base64)
if (msg.payload && msg.payload.message && msg.payload.message.data) {

    const dataEncoded = msg.payload.message.data;
    const dataDecoded = Buffer.from(dataEncoded, 'base64').toString('utf-8');

    // Converte a string JSON para objeto
    const eventData = JSON.parse(dataDecoded);

    // Armazena informações importantes
    msg.emailAddress = eventData.emailAddress;
    msg.historyId = eventData.historyId;

    return msg;

} else {

    node.warn("Payload recebido em formato inválido do PubSub");
    return null;

}
```

---

## Resultado Esperado

Ao término desta etapa, o fluxo possuirá duas informações fundamentais:

| Campo | Descrição |
|--------|-----------|
| `msg.emailAddress` | Conta Gmail que recebeu o novo e-mail. |
| `msg.historyId` | Identificador indicando que houve alteração na caixa postal. |

Esses dados serão utilizados nas próximas consultas à Gmail API.

---

# Etapa 3.3 — Buscar a Mensagem Completa na Gmail API

A notificação enviada pelo Pub/Sub informa apenas que houve alteração na caixa postal.

Ela **não contém o conteúdo do e-mail**.

Para obter remetente, assunto, anexos e corpo da mensagem, será necessário realizar consultas à **Gmail API**.

Esta etapa será dividida em duas chamadas HTTP:

1. Buscar a lista de mensagens.
2. Buscar os detalhes completos da mensagem encontrada.

---

## Passo 1 — Gerar o Token OAuth

Insira no fluxo o mesmo nó responsável pela geração do **Access Token OAuth 2.0** utilizado durante o envio de e-mails.

Este nó armazenará o token em:

```text
msg.token
```

---

## Passo 2 — Preparar a Consulta da Caixa Postal

Após o nó de geração do token, adicione um novo nó **Function**.

Nome sugerido:

```text
Prepara Consulta Gmail
```

Utilize o código abaixo:

```javascript
const token = msg.token;

// Consulta a última mensagem não lida
msg.url = "https://gmail.googleapis.com/gmail/v1/users/me/messages?maxResults=1&q=is:unread";
msg.method = "GET";

msg.headers = {
    "Authorization": "Bearer " + token,
    "Content-Type": "application/json"
};

return msg;
```

---

## Passo 3 — Executar a Consulta

Conecte o nó anterior a um **HTTP Request** configurado para utilizar os valores presentes em:

- `msg.url`
- `msg.method`
- `msg.headers`

Esta requisição retornará uma lista contendo os identificadores das mensagens encontradas.

Exemplo de retorno:

```json
{
  "messages": [
    {
      "id": "18f9ab3d...",
      "threadId": "18f9ab3d..."
    }
  ]
}
```

---

## Passo 4 — Buscar os Detalhes da Mensagem

Adicione um novo nó **Function** após o primeiro **HTTP Request**.

Nome sugerido:

```text
Buscar Conteúdo do E-mail
```

Utilize o código abaixo:

```javascript
// Recupera o ID da mensagem retornada

const messages = msg.payload.messages;

if (messages && messages.length > 0) {

    const messageId = messages[0].id;

    // Consulta o conteúdo completo da mensagem

    msg.url = `https://gmail.googleapis.com/gmail/v1/users/me/messages/${messageId}?format=full`;

    msg.method = "GET";

    msg.headers = {
        "Authorization": "Bearer " + msg.token
    };

    return msg;

} else {

    return null;

}
```

---

## Passo 5 — Consultar a Gmail API

Conecte esta função a um novo nó **HTTP Request**.

A resposta desta requisição conterá todas as informações da mensagem, incluindo:

- Remetente (**From**)
- Destinatário (**To**)
- Assunto (**Subject**)
- Data de envio
- Corpo do e-mail
- Corpo em HTML
- Corpo em texto simples
- Anexos (quando existirem)
- Identificadores da conversa (Thread ID)

Todas essas informações estarão disponíveis em:

```text
msg.payload
```

e poderão ser utilizadas pelo restante do fluxo para localizar o ticket correspondente, interpretar a resposta do cliente e iniciar o processamento automático da mensagem.

# Parte 4 — Ativar o Monitoramento (Comando `watch`)

A Gmail API utiliza o método **`watch`** para informar ao Google que uma determinada caixa postal deve ser monitorada.

Esse registro possui validade limitada e **expira periodicamente (até 7 dias)**. Por esse motivo, é necessário renová-lo automaticamente antes do vencimento.

A recomendação é executar a renovação a cada **5 dias**, garantindo que o monitoramento permaneça ativo sem interrupções.

---

# Fluxo de Renovação

```text
Inject (a cada 5 dias)
        │
        ▼
Gerar Token OAuth
        │
        ▼
Function (Watch)
        │
        ▼
HTTP Request
        │
        ▼
Debug
```

---

# Passo 1 — Criar o Nó Inject

Adicione um nó **Inject** ao fluxo.

Configure-o para:

- Executar automaticamente a cada **5 dias**.
- Permitir execução manual através do botão do próprio nó.

Este nó será responsável por renovar o registro de monitoramento da caixa postal.

---

# Passo 2 — Gerar o Token OAuth

Conecte o nó **Inject** ao mesmo fluxo responsável por gerar o **Access Token OAuth 2.0** utilizado nas demais chamadas da Gmail API.

O token deverá ser armazenado em:

```text
msg.token
```

---

# Passo 3 — Criar a Requisição `watch`

Após o nó de geração do token, adicione um nó **Function**.

Nome sugerido:

```text
Ativar Watch Gmail
```

Utilize o código abaixo:

```javascript
const token = msg.token;

msg.url = "https://gmail.googleapis.com/gmail/v1/users/me/watch";
msg.method = "POST";

msg.headers = {
    "Authorization": "Bearer " + token,
    "Content-Type": "application/json"
};

// Substitua pelo Topic criado no Google Cloud

msg.payload = {
    "topicName": "projects/SEU_PROJECT_ID/topics/gmail-incoming-emails",
    "labelIds": [
        "INBOX"
    ]
};

return msg;
```

---

## Configuração do Payload

O corpo enviado para a Gmail API contém dois parâmetros principais.

| Campo | Descrição |
|--------|-----------|
| `topicName` | Nome completo do tópico criado no Google Cloud Pub/Sub. |
| `labelIds` | Define quais categorias da caixa postal serão monitoradas. Neste exemplo, apenas a Caixa de Entrada (`INBOX`). |

---

# Passo 4 — Executar a Requisição

Conecte a função a um nó **HTTP Request**.

Após concluir a configuração:

1. Clique no botão do nó **Inject**.
2. Aguarde a execução da requisição.

Esta será a primeira ativação do monitoramento da caixa postal.

Posteriormente, a renovação ocorrerá automaticamente conforme a programação definida no nó **Inject**.

---

# Resultado Esperado

Caso a configuração esteja correta, a Gmail API responderá com **HTTP 200 OK**.

A resposta será semelhante ao exemplo abaixo:

```json
{
  "historyId": "123456789",
  "expiration": "1784556123456"
}
```

### Campos retornados

| Campo | Descrição |
|--------|-----------|
| `historyId` | Identificador inicial da caixa postal a partir do qual o Gmail começará a enviar notificações. |
| `expiration` | Data e hora (Unix Timestamp em milissegundos) em que o registro `watch` expirará. |

A presença do campo **`expiration`** indica que o monitoramento foi registrado com sucesso.

---

# Checklist de Validação

Antes de colocar a integração em produção, confirme todos os itens abaixo.

- [ ] A API **Cloud Pub/Sub** foi ativada no Google Cloud Platform.
- [ ] O tópico **gmail-incoming-emails** foi criado.
- [ ] A conta **gmail-api-push@system.gserviceaccount.com** recebeu a permissão **Pub/Sub Publisher**.
- [ ] A **Push Subscription** aponta corretamente para a URL pública do Node-RED.
- [ ] O endpoint HTTP do Node-RED responde com **HTTP 200 OK**.
- [ ] O comando **`watch`** foi executado com sucesso.
- [ ] A resposta da Gmail API retornou um código **HTTP 200**.
- [ ] A resposta contém o campo **`expiration`**.
- [ ] Um e-mail de teste foi enviado para a caixa monitorada.
- [ ] A notificação chegou ao endpoint do Node-RED.
- [ ] O conteúdo completo da mensagem foi recuperado através da Gmail API.
- [ ] O fluxo processou corretamente a mensagem recebida.

---

# Resultado Final

Após todas as etapas concluídas, a integração estará operando em tempo real.

O fluxo de funcionamento será o seguinte:

```text
Novo E-mail
        │
        ▼
Gmail API
        │
        ▼
Google Cloud Pub/Sub
        │
        ▼
Push Subscription
        │
        ▼
Webhook (Node-RED)
        │
        ▼
Decodifica Evento
        │
        ▼
Consulta Gmail API
        │
        ▼
Processa Ticket
```

A partir desse momento, qualquer novo e-mail recebido na caixa monitorada será entregue automaticamente ao Node-RED, sem necessidade de consultas periódicas (Polling), proporcionando uma arquitetura orientada a eventos (**Event-Driven**) com menor consumo de recursos e menor tempo de resposta.
