# Arquitetura de Integração: Envio de E-mails via Gmail API (Node-RED)

## 1. Visão Geral

Esta integração é responsável pelo envio automático de e-mails do sistema, como:

- Notificações de chamados;
- Alertas do sistema;
- Respostas de tickets;
- Confirmações automáticas.

O envio é realizado através da **Gmail API**, utilizando autenticação segura por **OAuth 2.0** com uma **Conta de Serviço (Service Account)**.

Diferentemente do protocolo SMTP tradicional, que depende de autenticação por usuário e senha e pode sofrer com bloqueios de portas, redefinições de credenciais e limitações do provedor, a Gmail API utiliza autenticação baseada em **JWT (JSON Web Token)**, proporcionando maior segurança, confiabilidade e desempenho.

---

# 2. Arquitetura da Solução

## Fluxo de Funcionamento

```text
Evento Interno / CRM
        │
        ▼
Preparação da Mensagem (MIME)
        │
        ▼
Codificação Base64URL
        │
        ▼
Geração do Token OAuth (JWT)
        │
        ▼
HTTP Request (POST Gmail API)
        │
        ▼
Registro de Envio (Log)
```

---

## Funcionamento Detalhado

### 1. Recebimento do Evento

O fluxo é iniciado por algum evento interno do sistema, como:

- Atualização de um ticket;
- Criação de um chamado;
- Envio de notificações automáticas;
- Processos internos do CRM.

O Node-RED recebe os dados necessários para composição do e-mail, incluindo:

- Destinatário;
- Assunto;
- Corpo da mensagem (texto simples ou HTML).

---

### 2. Montagem da Mensagem MIME

Antes do envio, a mensagem é convertida para o padrão **RFC 2822**, também conhecido como formato **MIME (Multipurpose Internet Mail Extensions)**.

São adicionados automaticamente os cabeçalhos necessários, como:

- Destinatário (`To`);
- Assunto (`Subject`);
- Tipo de conteúdo (`Content-Type`);
- Versão MIME (`MIME-Version`).

Este formato é o padrão utilizado pelos servidores de e-mail para representar mensagens eletrônicas.

---

### 3. Codificação Base64URL

Após a montagem da mensagem MIME, todo o conteúdo é convertido para **Base64URL Safe**.

A Gmail API não aceita o conteúdo MIME em texto puro.

Ela exige que a mensagem seja enviada codificada utilizando Base64URL, que difere do Base64 tradicional pelas seguintes substituições:

| Base64 | Base64URL |
|---------|-----------|
| `+` | `-` |
| `/` | `_` |
| `=` | Removido |

Essa codificação garante compatibilidade com transmissões HTTP e URLs.

---

### 4. Geração do Token OAuth

Antes do envio da mensagem, o Node-RED gera um **Access Token OAuth 2.0** utilizando uma **Conta de Serviço**.

A autenticação é realizada através da biblioteca **google-auth-library**, utilizando o mecanismo **JWT**.

Durante esse processo:

- A chave privada da Conta de Serviço assina a requisição.
- O Google valida a assinatura.
- Um Access Token temporário é emitido.
- A Conta de Serviço atua em nome da caixa de e-mail configurada (**Domain-Wide Delegation**).

Nesta integração é utilizado o escopo:

```text
https://mail.google.com/
```

Este escopo concede permissão completa para operações na Gmail API.

---

### 5. Envio da Mensagem

Com o token OAuth obtido, o Node-RED realiza uma requisição HTTP para a Gmail API.

Endpoint utilizado:

```http
POST https://gmail.googleapis.com/gmail/v1/users/me/messages/send
```

O corpo da requisição contém a mensagem MIME codificada em Base64URL.

Após o processamento, a Gmail API retorna um objeto JSON contendo informações da mensagem enviada.

---

# 3. Estrutura do Fluxo no Node-RED

## Fluxo Geral

```text
Evento
   │
   ▼
Preparar MIME
   │
   ▼
Codificar Base64URL
   │
   ▼
Gerar Token OAuth
   │
   ▼
HTTP Request
   │
   ▼
Log de Envio
```

---

# 3.1 Nó Function — Preparação e Codificação MIME

Este nó é responsável por converter os dados recebidos em uma mensagem compatível com o padrão **RFC 2822** e codificá-la em **Base64URL**, conforme exigido pela Gmail API.

### Payload esperado

```text
msg.emailData
```

Campos utilizados:

| Campo | Descrição |
|--------|-----------|
| `to` | Destinatário da mensagem. |
| `subject` | Assunto do e-mail. |
| `body` | Conteúdo da mensagem (HTML ou texto). |

### Código JavaScript

```javascript
// Exemplo de payload esperado na entrada: msg.emailData

const to = msg.emailData.to || "cliente@exemplo.com.br";
const subject = msg.emailData.subject || "Atualização no seu Chamado";
const body = msg.emailData.body || "Sua solicitação foi processada com sucesso.";

// Montagem do cabeçalho RFC 2822

const str = [
    `To: ${to}`,
    `Subject: =?utf-8?B?${Buffer.from(subject).toString('base64')}?=`,
    'Content-Type: text/html; charset=utf-8',
    'MIME-Version: 1.0',
    '',
    body
].join('\n');

// Conversão para Base64URL

const base64Encoded = Buffer.from(str)
    .toString('base64')
    .replace(/\+/g, '-')
    .replace(/\//g, '_')
    .replace(/=+$/, '');

msg.payload = {
    raw: base64Encoded
};

return msg;
```

---

## Funcionamento

Este nó executa as seguintes etapas:

1. Recebe os dados do e-mail.
2. Monta o cabeçalho MIME.
3. Codifica o assunto em UTF-8.
4. Converte toda a mensagem para Base64.
5. Adapta o Base64 para o formato Base64URL.
6. Armazena o resultado em:

```text
msg.payload.raw
```

---

# 3.2 Nó Function — Gerar Token de Envio

Este nó gera um **Access Token OAuth 2.0** utilizando a Conta de Serviço configurada no Google Cloud.

A autenticação utiliza o mecanismo **JWT**, permitindo que a Conta de Serviço represente a caixa de e-mail configurada através do recurso **Domain-Wide Delegation**.

### Código JavaScript

```javascript
const { JWT } = global.get('googleAuth');

const credentials = {
    client_email: "nodered-apiemail@melodic-zoo-440012-g4.iam.gserviceaccount.com",
    private_key: "-----BEGIN PRIVATE KEY-----\nSUA_CHAVE_PRIVADA_AQUI\n-----END PRIVATE KEY-----\n"
};

const client = new JWT({
    email: credentials.client_email,
    key: credentials.private_key,
    scopes: [
        'https://mail.google.com/'
    ],
    subject: 'caio@vipsolutions.com.br'
});

try {

    const tokens = await client.getAccessToken();

    msg.headers = {
        "Authorization": `Bearer ${tokens.token}`,
        "Content-Type": "application/json"
    };

    return msg;

} catch (error) {

    node.error("Erro ao gerar Token de Envio: " + error.message, msg);
    return null;

}
```

---

## Funcionamento

Este nó executa as seguintes operações:

- Carrega as credenciais da Conta de Serviço.
- Assina um JWT utilizando a chave privada.
- Solicita um Access Token ao Google.
- Armazena os cabeçalhos HTTP necessários em:

```text
msg.headers
```

---

# 3.3 Nó HTTP Request — Envio para a Gmail API

Após a geração do Access Token, o fluxo realiza a chamada à Gmail API.

## Configuração

| Propriedade | Valor |
|------------|-------|
| Método | POST |
| URL | `https://gmail.googleapis.com/gmail/v1/users/me/messages/send` |
| Tipo de retorno | JSON |

O corpo da requisição contém:

```json
{
    "raw": "BASE64URL..."
}
```

Após o processamento, a Gmail API retorna um objeto JSON contendo o identificador da mensagem enviada.

---

# 4. Padrões de Segurança e Boas Práticas

## Escopo OAuth Padronizado

Toda a integração utiliza o escopo:

```text
https://mail.google.com/
```

Esse escopo foi previamente autorizado no **Google Workspace Admin Console** através do mecanismo de **Domain-Wide Delegation**, permitindo que a Conta de Serviço atue em nome da caixa de e-mail configurada.

A utilização de um único escopo reduz inconsistências de autenticação e evita falhas causadas por divergência de permissões.

---

## Gravação Automática na Pasta "Enviados"

Mensagens enviadas através da Gmail API são armazenadas automaticamente na pasta **Sent (Enviados)** da caixa de e-mail utilizada como remetente.

Isso permite:

- Auditoria das mensagens enviadas.
- Rastreabilidade.
- Consulta diretamente pela interface do Gmail.
- Integração com demais recursos da plataforma Google Workspace.

---

## Tratamento de Caracteres UTF-8

O assunto da mensagem é codificado utilizando UTF-8 conforme especificação RFC 2047.

Além disso, o corpo da mensagem é enviado com o cabeçalho:

```text
Content-Type: text/html; charset=utf-8
```

Essa configuração garante a correta exibição de caracteres especiais, como:

- Acentos;
- Cedilha (ç);
- Símbolos;
- Caracteres internacionais.

---

## Autenticação sem Senhas

Toda a autenticação é realizada através de **OAuth 2.0** utilizando uma **Conta de Serviço**.

Dessa forma:

- Não há armazenamento de senhas da caixa postal.
- Não é necessário utilizar autenticação SMTP.
- O acesso é concedido por meio de tokens temporários emitidos pelo Google.
- As credenciais permanecem protegidas pela chave privada da Conta de Serviço.
