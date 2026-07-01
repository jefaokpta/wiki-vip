# Integração Agendor

## Visão Geral

Esta documentação descreve o fluxo de integração utilizado para anexar gravações de chamadas ao Agendor utilizando sua API REST.

> **Importante**
>
> Todos os tokens, IDs, URLs assinadas, números de telefone e identificadores apresentados nesta documentação são apenas exemplos e devem ser substituídos pelos valores do seu ambiente.

---

# Fluxo da Integração

```text
Consultar usuário (opcional)
        │
        ▼
Consultar chamada VoIP
        │
        ▼
Solicitar URL temporária para upload
        │
        ▼
Enviar arquivo para AWS S3
        │
        ▼
Criar tarefa com o áudio anexado
```

---

# Autenticação

Todas as chamadas para a API do Agendor utilizam:

```
Authorization: Token <SEU_TOKEN>
Content-Type: application/json
```

---

# Endpoint 1 — Listar Usuários

**GET** `/v3/users`

Permite consultar os usuários cadastrados para obter seus respectivos IDs.

---

# Endpoint 2 — Consultar Chamada VoIP

**GET** `/v3/voip_calls`

## Query Parameters

| Campo | Descrição |
|--------|-----------|
| phone_number | Número utilizado na pesquisa |

Exemplo:

```
GET /v3/voip_calls?phone_number=<NUMERO>
```

Objetivo: localizar o contato relacionado ao número telefônico.

---

# Endpoint 3 — Solicitar URL de Upload

**POST** `/v3/attachments/presigned_urls`

Body:

```json
{
  "filenames": [
    "gravacao.wav"
  ]
}
```

A resposta retorna uma URL temporária e uma `temporary_key`.

Esses dados serão utilizados no upload do arquivo.

---

# Endpoint 4 — Upload do Arquivo

A URL retornada aponta para um bucket temporário da AWS S3.

Método:

```
PUT
```

Header:

```
Content-Type: application/octet-stream
```

Body:

Arquivo de áudio (.wav)

Observações:

- Não enviar Authorization.
- A URL possui validade limitada.
- O upload deve ocorrer antes da expiração.

---

# Endpoint 5 — Criar Tarefa

**POST** `/v3/people/{personId}/tasks`

Exemplo:

```json
{
  "text":"Ligação realizada",
  "attachments":[
    {
      "type":"document",
      "filename":"gravacao.wav",
      "temporary_key":"<TEMPORARY_KEY>",
      "title":"Gravação",
      "description":"Áudio da chamada"
    }
  ],
  "assigned_users":[12345],
  "due_date":"2026-01-01T10:00:00Z",
  "type":"LIGACAO"
}
```

## Campos

| Campo | Descrição |
|--------|-----------|
| text | Descrição da tarefa |
| attachments | Arquivos anexados |
| filename | Nome do arquivo enviado |
| temporary_key | Chave retornada no upload |
| title | Nome exibido no Agendor |
| description | Descrição do anexo |
| assigned_users | IDs dos responsáveis |
| due_date | Data limite |
| type | Tipo da tarefa |

---

# Fluxo Completo

1. Obter o ID do usuário (quando necessário).
2. Consultar o número telefônico utilizando `voip_calls`.
3. Solicitar uma URL temporária para upload.
4. Enviar o arquivo WAV para a URL recebida.
5. Utilizar a `temporary_key` retornada para criar uma tarefa contendo o anexo.

---

# Boas Práticas

- Nunca armazene URLs pré-assinadas.
- Nunca compartilhe tokens de autenticação.
- Realize o upload imediatamente após gerar a URL.
- Trate respostas de erro da API.
- Utilize HTTPS em todas as requisições.

---

# Pré-requisitos

- Token válido da API Agendor.
- Permissão para criação de tarefas.
- Permissão para upload de anexos.
- Arquivos de áudio em formato suportado (.wav).

---

# Referência dos Endpoints

| Método | Endpoint | Finalidade |
|--------|----------|------------|
| GET | /v3/users | Listar usuários |
| GET | /v3/voip_calls | Consultar chamadas |
| POST | /v3/attachments/presigned_urls | Gerar URL de upload |
| PUT | URL temporária AWS | Enviar arquivo |
| POST | /v3/people/{personId}/tasks | Criar tarefa com anexo |
