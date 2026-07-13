# Convenções de nomenclatura

## 1. O que é Markdown

Markdown é uma linguagem de marcação leve utilizada para criar documentos utilizando apenas texto.

Seu principal objetivo é permitir que qualquer pessoa consiga escrever documentação de forma simples, sem precisar utilizar HTML.

Um arquivo Markdown possui a extensão:

~~~text
.md
~~~

### Exemplos

~~~text
README.md

api-clientes.md

instalacao-servidor.md
~~~

---

## 2. Estrutura de Títulos

Utilize o caractere `#` seguido de um espaço para criar títulos.

Quanto maior a quantidade de caracteres `#`, menor será o nível do título.

### Exemplos

~~~md
# Título Principal

## Seção

### Subseção

#### Tópico

##### Item

###### Observação
~~~

### Boas práticas

- Utilize apenas um título **H1 (`#`)** por documento.
- Utilize **H2 (`##`)** para separar grandes seções.
- Evite pular níveis de títulos (exemplo: `H2 → H4`).

---

# 3. Formatação de Texto

## Negrito

Utilize dois asteriscos (`**`) antes e depois do texto.

### Exemplo

~~~md
**Texto**
~~~

Resultado:

**Texto**

---

## Itálico

Utilize um asterisco (`*`) antes e depois do texto.

### Exemplo

~~~md
*Texto*
~~~

Resultado:

*Texto*

---

## Negrito + Itálico

Utilize três asteriscos (`***`) antes e depois do texto.

### Exemplo

~~~md
***Texto***
~~~

Resultado:

***Texto***

---

## Texto Riscado

Utilize dois tils (`~~`) antes e depois do texto.

### Exemplo

~~~md
~~Texto~~
~~~

Resultado:

~~Texto~~

# 4. Listas

## Lista não ordenada

Utilize o caractere `-` para criar listas sem ordem definida.

### Exemplo

~~~md
- Item A
- Item B
- Item C
~~~

Resultado:

- Item A
- Item B
- Item C

---

## Lista numerada

Utilize números seguidos de ponto (`.`).

### Exemplo

~~~md
1. Primeiro passo

2. Segundo passo

3. Terceiro passo
~~~

Resultado:

1. Primeiro passo
2. Segundo passo
3. Terceiro passo

---

## Checklist

Ideal para documentar procedimentos técnicos, tarefas e validações.

### Exemplo

~~~md
- [x] Banco atualizado

- [x] Docker iniciado

- [ ] Validar API
~~~

Resultado:

- [x] Banco atualizado
- [x] Docker iniciado
- [ ] Validar API

---

# 5. Links

## Link externo

Utilizado para apontar para páginas externas.

### Exemplo

~~~md
[Portal do Cliente](https://empresa.com)
~~~

Resultado:

[Portal do Cliente](https://empresa.com)

---

## Link para outro arquivo

Utilizado para navegar entre documentos Markdown dentro do mesmo projeto.

### Exemplo

~~~md
[Instalação](instalacao.md)
~~~

Resultado:

[Instalação](instalacao.md)

---

## Link interno (Âncora)

Utilizado para navegar para uma seção dentro do próprio documento.

### Exemplo

~~~md
[Ir para Boas Práticas](#13-boas-práticas)
~~~

Resultado:

[Ir para Boas Práticas](#13-boas-práticas)

---

# 6. Imagens

Utilize o formato:

~~~md
![Arquitetura](imagens/arquitetura.png)
~~~

### Formatos suportados

- PNG
- JPG
- JPEG
- GIF
- SVG

---

# 7. Citações

Utilize o caractere `>` para destacar observações importantes.

### Exemplo

~~~md
> Nunca documente senhas em arquivos Markdown.
~~~

Resultado:

> Nunca documente senhas em arquivos Markdown.

---

# 8. Linha Horizontal

Utilize três hífens (`---`) para separar grandes blocos de conteúdo.

### Exemplo

~~~md
---
~~~

Resultado:

---

# 9. Tabelas

Tabelas são ideais para documentar estruturas de dados, APIs, parâmetros e configurações.

### Exemplo

~~~md
| Campo | Tipo | Obrigatório | Descrição |
| :--- | :---: | :---: | ---: |
| id | UUID | Sim | Identificador |
| nome | String | Sim | Nome completo |
| status | String | Não | Situação |
~~~

Resultado:

| Campo | Tipo | Obrigatório | Descrição |
| :--- | :---: | :---: | ---: |
| id | UUID | Sim | Identificador |
| nome | String | Sim | Nome completo |
| status | String | Não | Situação |

# 10. Blocos de Código

## Código Inline

Utilizado para destacar pequenos trechos de código, comandos ou nomes de variáveis dentro de um texto.

### Exemplo

~~~md
Utilize o comando `docker compose up`.
~~~

Resultado:

Utilize o comando `docker compose up`.

---

## Código Multilinhas

Utilize três crases (` ``` `) para criar blocos de código.

É recomendado informar a linguagem para melhorar a formatação e a leitura.

### Exemplo

~~~md
```javascript
console.log("Olá");
```
~~~

Resultado:

```javascript
console.log("Olá");
```

---

## Linguagens suportadas

Algumas linguagens comuns utilizadas em documentação:

- bash
- json
- yaml
- javascript
- typescript
- python
- php
- java
- sql
- dockerfile
- html
- css
- xml

---

# 11. Recursos Avançados

## Seções Retráteis

Permitem esconder conteúdos extensos, detalhes técnicos ou informações opcionais.

### Exemplo

~~~html
<details>

<summary>Mostrar detalhes</summary>

Conteúdo...

</details>
~~~

Resultado:

<details>

<summary>Mostrar detalhes</summary>

Conteúdo...

</details>

---

## Notas de Rodapé

Utilizadas para adicionar referências ou explicações adicionais ao final do documento.

### Exemplo

~~~md
Sistema baseado em gRPC[^1].

[^1]: Framework criado pelo Google.
~~~

Resultado:

Sistema baseado em gRPC[^1].

[^1]: Framework criado pelo Google.

---

# Alertas

Alguns renderizadores Markdown, como GitHub e GitLab, suportam blocos de alerta utilizando a sintaxe:

---

## Nota

Utilizado para informações importantes.

### Exemplo

~~~md
> [!NOTE]
>
> Informação importante.
~~~

Resultado:

> [!NOTE]
>
> Informação importante.

---

## Dica

Utilizado para recomendações e boas práticas.

### Exemplo

~~~md
> [!TIP]
>
> Boa prática.
~~~

Resultado:

> [!TIP]
>
> Boa prática.

---

## Aviso

Utilizado para alertar sobre possíveis problemas.

### Exemplo

~~~md
> [!WARNING]
>
> Pode causar falhas.
~~~

Resultado:

> [!WARNING]
>
> Pode causar falhas.

---

## Cuidado

Utilizado para indicar riscos ou ações potencialmente perigosas.

### Exemplo

~~~md
> [!CAUTION]
>
> Risco de perda de dados.
~~~

Resultado:

> [!CAUTION]
>
> Risco de perda de dados.

# 13. Guia de Boas Práticas

## Escape de caracteres

Caso seja necessário exibir caracteres especiais do Markdown como texto, utilize uma barra invertida (`\`) antes do caractere.

### Exemplos

~~~md
\#

\*

\`

\_
~~~

Resultado:

\#

\*

\`

\_

---

## Quebra de linha

O Markdown normalmente ignora quebras de linha simples dentro de um parágrafo.

Para forçar uma quebra de linha, utilize:

### Opção 1 - Dois espaços no final da linha

~~~md
Primeira linha.  
Segunda linha.
~~~

### Opção 2 - Tag HTML `<br>`

~~~html
Primeira linha.<br>
Segunda linha.
~~~

---

## Organização

Prefira documentos menores e mais específicos.

Documentos muito grandes dificultam a manutenção e localização das informações.

### Evite

~~~text
Sistema.md
~~~

### Prefira

~~~text
Sistema

├── API.md

├── Banco.md

├── Docker.md

├── Deploy.md

├── FAQ.md
~~~

---

## Documente pensando em outra pessoa

Uma boa documentação deve permitir que outra pessoa compreenda o funcionamento sem depender de conhecimento prévio.

Sempre responda:

- O que faz?
- Quando usar?
- Como usar?
- Exemplo.
- Erros comuns.

---

# 14. Convenções de Arquivos

Os arquivos Markdown devem seguir um padrão de nomenclatura.

## Sempre utilize

- Letras minúsculas.
- Hífens (`-`) em vez de espaços.
- Sem acentos.
- Sem caracteres especiais.

### Exemplos corretos

~~~text
instalacao-servidor.md

api-clientes.md

configuracao-nginx.md

manual-node-red.md
~~~

---

## Evite

~~~text
Instalação.doc

Meu Arquivo.md

API Clientes.md

Configuração.md
~~~

---

# Considerações Finais

Seguir uma padronização de Markdown facilita a manutenção da documentação, melhora a organização do conhecimento e permite que diferentes pessoas contribuam de forma consistente para o projeto.

# Utilizando o Mermaid

Markdown é atualmente um dos padrões de documentação mais utilizados pelas principais plataformas de desenvolvimento do mercado.

Além de ser simples de escrever, oferece excelente integração com controle de versão, facilita revisões utilizando Git e permite criar documentação técnica rica, incluindo:

- Tabelas.
- Exemplos de código.
- Diagramas.
- Fluxos de processo.

---

## Boas práticas de documentação

Sempre que possível, mantenha a documentação:

- Objetiva.
- Atualizada.
- Padronizada.
- Acompanhada de exemplos práticos.
- Organizada em arquivos pequenos e específicos.

---

# Mermaid

Os "comandos" do Mermaid são, na verdade, **palavras-chave da linguagem**.

O Mermaid possui diversos tipos de diagramas. Abaixo estão apresentados os exemplos mais utilizados em documentação técnica.

---

# 1. Fluxograma (Flowchart)

Utilizado para representar processos, decisões e fluxos de execução.

## Código Mermaid

```text
flowchart TD
    A[Início] --> B[Login]
    B --> C{Senha correta?}
    C -->|Sim| D[Página Inicial]
    C -->|Não| E[Erro]
    E --> B
```

## Resultado

~~~mermaid
flowchart TD

    A[Início] --> B[Login]
    B --> C{Senha correta?}

    C -->|Sim| D[Página Inicial]
    C -->|Não| E[Erro]

    E --> B
~~~

---

## Forma simplificada

Também é possível criar fluxos utilizando a sintaxe:

## Código Mermaid

```text
graph LR
    A --> B --> C
```

## Resultado

~~~mermaid
graph LR

    A --> B --> C
~~~

---

## Direções do fluxo

A direção define como os elementos serão organizados visualmente.

| Código | Nome | Direção |
|---|---|---|
| TD | Top Down | Cima para baixo |
| TB | Top Bottom | Equivalente ao TD |
| LR | Left Right | Esquerda para direita |
| RL | Right Left | Direita para esquerda |
| BT | Bottom Top | Baixo para cima |

### Exemplos

~~~mermaid
flowchart LR

    A[Início] --> B[Processo] --> C[Fim]
~~~

~~~mermaid
flowchart TD

    A[Início]
    B[Processo]
    C[Fim]

    A --> B --> C
~~~

# 2. Sequence Diagram

Utilizado para representar a comunicação e troca de mensagens entre sistemas, serviços ou componentes.

## Código Mermaid

```text
sequenceDiagram
    participant Cliente
    participant API
    participant Banco
    Cliente->>API: POST /login
    API->>Banco: Consulta usuário
    Banco-->>API: Dados
    API-->>Cliente: Token JWT
```

## Resultado

~~~mermaid
sequenceDiagram

    participant Cliente
    participant API
    participant Banco

    Cliente->>API: POST /login
    API->>Banco: Consulta usuário
    Banco-->>API: Dados
    API-->>Cliente: Token JWT
~~~

---

# 3. Class Diagram

Utilizado principalmente para documentação técnica, modelagem de objetos e representação de estruturas de classes.

## Código Mermaid

```text
classDiagram
class Cliente{
    +int id
    +string nome
    +salvar()
}
class Pedido{
    +int numero
}
Cliente --> Pedido
```

## Resultado

~~~mermaid
classDiagram

    class Cliente{
        +int id
        +string nome
        +salvar()
    }

    class Pedido{
        +int numero
    }

    Cliente --> Pedido
~~~

---

# 4. State Diagram

Utilizado para representar os estados de um processo e suas transições.

## Código Mermaid

```text
stateDiagram-v2
[*] --> Aguardando
Aguardando --> Processando
Processando --> Finalizado
Processando --> Erro
Finalizado --> [*]
Erro --> [*]
```

## Resultado

~~~mermaid
stateDiagram-v2

    [*] --> Aguardando

    Aguardando --> Processando

    Processando --> Finalizado
    Processando --> Erro

    Finalizado --> [*]
    Erro --> [*]
~~~

---

# 5. ER Diagram (Banco de Dados)

Utilizado para documentar estruturas de banco de dados, tabelas e relacionamentos entre entidades.

## Código Mermaid

```text
erDiagram
CLIENTE ||--o{ PEDIDO : possui
CLIENTE {
    int id
    string nome
}
PEDIDO {
    int id
    decimal valor
}
```

## Resultado

~~~mermaid
erDiagram

    CLIENTE ||--o{ PEDIDO : possui

    CLIENTE {
        int id
        string nome
    }

    PEDIDO {
        int id
        decimal valor
    }
~~~

---

# 6. Gantt

Utilizado para criar cronogramas de projetos, acompanhamento de tarefas e planejamento de entregas.

## Código Mermaid

```text
gantt
title Projeto
dateFormat YYYY-MM-DD
section Backend
API :2026-07-01,5d
Banco :3d
section Frontend
Tela Login :4d
```

## Resultado

~~~mermaid
gantt

    title Projeto

    dateFormat YYYY-MM-DD

    section Backend

    API :2026-07-01,5d

    Banco :3d

    section Frontend

    Tela Login :4d
~~~

# 7. Journey

Utilizado para criar mapas da jornada do usuário, mostrando etapas, ações e experiências durante um processo.

## Código Mermaid

```text
journey
    title Compra Online
    section Escolha
      Pesquisar produto: 5: Cliente
    section Compra
      Finalizar pedido: 4: Cliente
    section Entrega
      Receber produto: 5: Cliente
```

## Resultado

~~~mermaid
journey

    title Compra Online

    section Escolha
      Pesquisar produto: 5: Cliente

    section Compra
      Finalizar pedido: 4: Cliente

    section Entrega
      Receber produto: 5: Cliente
~~~

---

# 8. Git Graph

Utilizado para representar visualmente estratégias de versionamento Git, branches e merges.

Muito útil para explicar fluxos de desenvolvimento.

## Código Mermaid

```text
gitGraph
commit
commit
branch develop
checkout develop
commit
commit
checkout main
merge develop
```

## Resultado

~~~mermaid
gitGraph

    commit

    commit

    branch develop

    checkout develop

    commit

    commit

    checkout main

    merge develop
~~~

---

# 9. Pie Chart

Utilizado para representar proporções e distribuições de dados.

## Código Mermaid

```text
pie
title Chamados
"Resolvidos" : 80
"Abertos" : 15
"Pendentes" : 5
```

## Resultado

~~~mermaid
pie

    title Chamados

    "Resolvidos" : 80

    "Abertos" : 15

    "Pendentes" : 5
~~~

---

# 10. Timeline

Utilizado para representar eventos históricos, versões ou marcos de um projeto.

## Código Mermaid

```text
timeline
title Histórico
2024 : Sistema criado
2025 : API integrada
2026 : Nova arquitetura
```

## Resultado

~~~mermaid
timeline

    title Histórico

    2024 : Sistema criado

    2025 : API integrada

    2026 : Nova arquitetura
~~~

---

# 11. Mindmap

Utilizado para organizar ideias, módulos e estruturas hierárquicas.

## Código Mermaid

```text
mindmap
  root((Projeto))
    Backend
      API
      Banco
    Infraestrutura
      Docker
      Nginx
```

## Resultado

~~~mermaid
mindmap
  root((Sistema))
    Backend
      API
      Banco
    Infraestrutura
      Docker
      Nginx
~~~

---

# Formatos de nós (caixas)

No Mermaid, é possível variar o formato visual dos elementos utilizando diferentes sintaxes.

| Sintaxe | Resultado |
|---|---|
| `A[Texto]` | Retângulo |
| `A(Texto)` | Cantos arredondados |
| `A((Texto))` | Círculo |
| `A{Texto}` | Losango (decisão) |
| `A>Texto]` | Assimétrico |
| `A[/Texto/]` | Paralelogramo |
| `A[\Texto\]` | Paralelogramo invertido |
| `A[[Texto]]` | Sub-rotina |

---

# Tipos de seta

O Mermaid permite diferentes tipos de conexões entre elementos.

| Sintaxe | Resultado |
|---|---|
| `-->` | Seta normal |
| `---` | Linha sem seta |
| `==>` | Seta grossa |
| `-.->` | Seta pontilhada |

---

## Texto nas setas

Também é possível adicionar informações nas conexões.

### Exemplo

~~~mermaid
flowchart LR

    A -->|Sucesso| B

    A -->|Erro| C
~~~

---

# Diagramas recomendados para documentação de APIs e integrações

Para documentar integrações como **Node-RED, Agendor, Tiflux, CRMs, webhooks e APIs**, os diagramas mais utilizados normalmente são:

## 1. Flowchart (`flowchart`)

Utilizado para:

- Fluxos de processamento.
- Regras de negócio.
- Processos internos.
- Rotinas de automação.

---

## 2. Sequence Diagram (`sequenceDiagram`)

Utilizado para:

- Comunicação entre sistemas.
- APIs.
- Webhooks.
- Integrações externas.

---

## 3. ER Diagram (`erDiagram`)

Utilizado para:

- Modelagem de banco de dados.
- Relacionamentos entre tabelas.
- Documentação de estruturas SQL.

---

## 4. Class Diagram (`classDiagram`)

Utilizado para:

- Estrutura de objetos.
- Classes.
- Modelos de software.

---

## 5. Git Graph (`gitGraph`)

Utilizado para:

- Estratégias de versionamento.
- Fluxos de branches.
- Processos de merge.

---

Com esses cinco tipos de diagramas é possível documentar praticamente toda a arquitetura de um sistema, incluindo:

- Fluxos de API.
- Integrações entre serviços.
- Estrutura de banco de dados.
- Processos técnicos.
- Estratégias de desenvolvimento.
