# API de Integração – Sistema VIP

## Visão geral

A API do Sistema VIP permite que aplicações externas (como CRMs e sistemas de atendimento) iniciem chamadas telefônicas através da funcionalidade **Click-to-Call**.

O fluxo é simples:

1. A aplicação envia uma requisição `POST` para o Sistema VIP.
2. O ramal (ou softphone) do operador toca.
3. Após o operador atender, o sistema realiza a discagem para o número informado.
4. Ao término da ligação, o Sistema VIP envia um `POST` para uma URL configurada pelo cliente contendo os dados da chamada.

> **Importante:** Os exemplos de código do cliente, ramal, URLs e demais identificadores apresentados nesta documentação são **meramente ilustrativos**. Cada ambiente possui suas próprias configurações.

---

# 1. Iniciar uma chamada

**Método:** `POST`

**Endpoint (exemplo):**

```text
https://callcenter.vipsolutions.com.br/services/call
```

## Parâmetros enviados

| Parâmetro | Descrição |
|-----------|-----------|
| `peer` | Código do cliente concatenado com o número do ramal que receberá a chamada. |
| `phone` | Número que será discado. |
| `callid` | Identificador da chamada no sistema externo (CRM). |

### Exemplo do campo `peer`

Código do cliente: `100`

Ramal: `1000`

Resultado:

```text
1001000
```

> O código do cliente utilizado acima é apenas um exemplo.

---

# 2. Retorno da chamada (Webhook)

Após o encerramento da ligação, o Sistema VIP enviará um **POST** para uma URL definida pelo cliente.

| Campo | Descrição | Formato |
|-------|-----------|----------|
| `calldate` | Data e hora de início | `Y-m-d H:i:s` |
| `src` | Ramal originador | Inteiro |
| `dst` | Número discado | Inteiro |
| `duration` | Tempo total (espera + conversação) | Segundos |
| `billsec` | Tempo de conversação | Segundos |
| `disposition` | Situação da chamada | Texto |
| `userfield` | Identificador da gravação | Texto |
| `callid` | Mesmo identificador enviado na abertura | Texto |
| `price` | Valor da chamada | Decimal |
| `accountcode` | Tipo da ligação | Texto |

### Valores de `disposition`

- ANSWER
- NO ANSWER
- BUSY
- CANCEL

### Valores de `accountcode`

- 1.00 — Entradas
- 2.00 — Fixo Local
- 3.00 — Fixo Brasil
- 5.00 — Móvel Local
- 6.00 — Móvel Brasil

---

# 3. Exemplo de envio

```php
$options = array(
    "http" => array(
        "method"  => "POST",
        "header"  => "Content-Type: application/json",
        "content" => json_encode($data)
    )
);
```

---

# 4. Campo `userfield`

O campo `userfield` identifica unicamente uma chamada e é utilizado para recuperar sua gravação.

Exemplo:

```text
20221031_131332_1001913_1142100101_1667232808
```

Os valores são separados por `_` (underline).

---

# 5. Recuperar gravação

**Método:** `GET`

Exemplo:

```text
https://callcenter.vipsolutions.com.br/services/record/{userfield}
```

Substitua `{userfield}` pelo identificador recebido no webhook.
