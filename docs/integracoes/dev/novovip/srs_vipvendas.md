# Especificações Gerais do Projeto

* [ ] O VIP está evoluindo de uma plataforma predominantemente de voz para uma plataforma de atendimento omnichannel.
* [ ] O Módulo de Vendas será um módulo próprio dentro dessa plataforma.
* [ ] Os canais inicialmente relevantes são telefonia e WhatsApp, mas a arquitetura deve permitir outros canais no futuro.
* [ ] O vendedor precisa conseguir trabalhar voz + mensagens dentro do mesmo contexto comercial.
* [ ] O atendimento deve poder evoluir para uma oportunidade de venda.
* [ ] O sistema precisa ter uma experiência semelhante a CRM + inbox omnichannel + pipeline, em vez de simplesmente ser uma tela de chamadas com um CRM acoplado.
* [ ] O documento será usado para discussão com a área comercial, portanto vou colocar bastante coisa que pode ser posteriormente removida.
* [ ] Vou incorporar tendências atuais como pipeline visual, automação, distribuição de leads, SLA, cadências, histórico unificado, lead scoring, IA, transcrição, análise de chamadas, templates, métricas de conversão e governança, mas sem assumir que tudo necessariamente entrará na primeira versão.

# # SRS — Módulo de Vendas VIP

**Software Requirements Specification — Especificação de Requisitos de Software**

**Produto:** VIP
**Módulo:** Vendas
**Versão:** 0.1 — Rascunho para Validação
**Status:** Em elaboração
**Responsável:** Equipe de Produto / Tecnologia
**Público:** Diretoria Comercial, Produto, Projetos, Desenvolvimento e Operações

---

# 1. Objetivo do documento

Este documento apresenta uma proposta inicial de **Especificação de Requisitos de Software (SRS)** para o futuro **Módulo de Vendas da plataforma VIP**.

O objetivo deste documento, neste estágio, não é estabelecer uma especificação definitiva, mas fornecer uma **base funcional abrangente para discussão, validação e refinamento junto à Diretoria Comercial e demais áreas envolvidas**.

A proposta considera a evolução do VIP de uma plataforma historicamente orientada à comunicação por voz para uma plataforma de **comunicação e relacionamento omnichannel**, capaz de centralizar telefonia, WhatsApp, atendimento e processos comerciais.

O Módulo de Vendas deverá transformar as interações realizadas pelos canais de comunicação em **processos comerciais estruturados**, permitindo que leads, contatos, conversas, ligações, oportunidades e vendas sejam acompanhados dentro de um mesmo ambiente.

---

# 2. Visão do produto

O Módulo de Vendas deverá funcionar como uma camada comercial integrada aos demais módulos do VIP.

A visão proposta é:

```text
                    ┌───────────────────────┐
                    │         VIP           │
                    │ Plataforma Omnichannel│
                    └───────────┬───────────┘
                                │
              ┌─────────────────┼─────────────────┐
              │                 │                 │
              ▼                 ▼                 ▼
        ┌───────────┐     ┌───────────┐     ┌───────────┐
        │ Telefonia │     │ WhatsApp  │     │ Atendimento│
        └─────┬─────┘     └─────┬─────┘     └─────┬─────┘
              │                 │                 │
              └─────────────────┼─────────────────┘
                                ▼
                    ┌───────────────────────┐
                    │     MÓDULO VENDAS     │
                    └───────────┬───────────┘
                                │
        ┌───────────────────────┼───────────────────────┐
        ▼                       ▼                       ▼
     Leads                Oportunidades             Clientes
        │                       │                       │
        └───────────────────────┼───────────────────────┘
                                ▼
                         Pipeline de Vendas
                                │
                                ▼
                         Venda / Conversão
```

O conceito central é que **a conversa não seja separada da venda**.

Uma ligação realizada pelo vendedor, uma conversa no WhatsApp, uma transferência de atendimento ou uma interação futura deverão poder fazer parte do histórico comercial do mesmo contato e, quando aplicável, da mesma oportunidade.

---

# 3. Motivação

O mercado de vendas está migrando de CRMs utilizados principalmente como bancos de dados para plataformas capazes de **participar ativamente do processo comercial**.

Em pesquisas recentes da Salesforce, equipes de vendas apontam agentes de IA como parte importante do crescimento e já estão utilizando IA em diferentes etapas do ciclo comercial. Pesquisas anteriores da empresa também indicaram que profissionais de vendas gastam uma parcela significativa do tempo em tarefas que não são efetivamente vender.

No mercado brasileiro, o WhatsApp possui importância ainda maior. Dados divulgados pela HubSpot em 2026 apontam que 79% dos times brasileiros pesquisados utilizam WhatsApp para prospecção, enquanto ligações telefônicas continuam relevantes.

Dessa forma, o VIP possui uma oportunidade estratégica:

> **Transformar comunicação em processo comercial mensurável.**

Em vez de apenas registrar que uma chamada ou mensagem aconteceu, o sistema deverá permitir responder:

* Quem é o cliente?
* Qual vendedor está responsável?
* De onde veio o lead?
* Qual produto ele procura?
* Em qual etapa da venda ele está?
* Qual foi a última interação?
* O que foi conversado?
* Qual é o próximo passo?
* Quanto vale essa oportunidade?
* Qual a probabilidade de fechamento?
* Há quanto tempo ela está parada?
* Quantas interações foram necessárias?
* Qual canal converte melhor?
* Quais vendedores possuem melhor desempenho?
* Quais oportunidades estão em risco?

---

# 4. Objetivos do Módulo

## 4.1 Objetivo principal

Disponibilizar uma plataforma de gestão comercial integrada aos canais de comunicação do VIP, permitindo gerenciar todo o ciclo de vendas desde a entrada do lead até a conversão.

## 4.2 Objetivos específicos

O módulo deverá buscar:

* Centralizar informações comerciais.
* Centralizar interações de voz e mensagens.
* Organizar leads e oportunidades.
* Permitir gestão visual do pipeline.
* Reduzir atividades administrativas do vendedor.
* Automatizar tarefas repetitivas.
* Melhorar o acompanhamento dos leads.
* Reduzir oportunidades esquecidas.
* Permitir acompanhamento de metas.
* Melhorar previsibilidade comercial.
* Registrar o histórico completo do relacionamento.
* Permitir análises de conversão.
* Integrar atendimento e vendas.
* Preparar a plataforma para recursos de Inteligência Artificial.
* Criar uma base para expansão futura da plataforma VIP.

---

# 5. Conceito central

O Módulo de Vendas deverá trabalhar com quatro conceitos principais:

```text
CONTATO
   │
   ├── Conversas
   ├── Ligações
   ├── Histórico
   └── Dados cadastrais
          │
          ▼
       LEAD
          │
          ▼
    QUALIFICAÇÃO
          │
          ▼
     OPORTUNIDADE
          │
          ▼
       PIPELINE
          │
          ▼
       NEGOCIAÇÃO
          │
          ▼
    VENDA / PERDA
```

Uma mesma pessoa ou empresa poderá possuir diversas oportunidades ao longo do tempo.

Exemplo:

> João da Empresa X entra em contato pelo WhatsApp solicitando informações sobre uma solução de telefonia.

O sistema deverá permitir:

1. Identificar ou criar o contato.
2. Criar o lead.
3. Registrar a origem.
4. Associar o vendedor.
5. Registrar a conversa.
6. Qualificar a oportunidade.
7. Criar uma oportunidade comercial.
8. Colocar a oportunidade no pipeline.
9. Registrar ligações e mensagens.
10. Registrar proposta.
11. Acompanhar follow-ups.
12. Fechar como ganha ou perdida.

---

# 6. Terminologia

## 6.1 Contato

Pessoa física relacionada à operação comercial.

## 6.2 Empresa / Conta

Organização à qual um ou mais contatos podem estar associados.

## 6.3 Lead

Potencial cliente ainda em processo de qualificação.

## 6.4 Oportunidade

Potencial negócio comercial identificado e qualificado.

## 6.5 Pipeline

Representação das etapas do processo comercial.

## 6.6 Etapa

Estado atual de uma oportunidade dentro do pipeline.

## 6.7 Atividade

Ação comercial planejada ou realizada, como:

* ligação;
* mensagem;
* reunião;
* tarefa;
* follow-up;
* envio de proposta.

## 6.8 Conversa

Conjunto de interações realizadas por determinado canal.

## 6.9 Conversão

Mudança do lead ou oportunidade para um estado comercial mais avançado, especialmente venda ganha.

---

# 7. Personas e perfis de utilização

O módulo deverá considerar diferentes perfis.

## 7.1 Vendedor

Responsável pela condução das oportunidades.

Necessidades:

* visualizar seus leads;
* realizar ligações;
* enviar mensagens;
* acompanhar conversas;
* criar oportunidades;
* atualizar etapas;
* agendar tarefas;
* acompanhar metas;
* consultar histórico.

## 7.2 Supervisor Comercial

Necessidades:

* acompanhar equipe;
* visualizar pipeline;
* distribuir leads;
* monitorar produtividade;
* acompanhar oportunidades;
* identificar gargalos;
* analisar conversão.

## 7.3 Gerente Comercial

Necessidades:

* visão global da operação;
* previsão de vendas;
* metas;
* indicadores;
* desempenho por equipe;
* desempenho por canal;
* análise de receita.

## 7.4 Administrador

Responsável pela configuração da plataforma.

Poderá administrar:

* usuários;
* equipes;
* permissões;
* pipelines;
* etapas;
* campos;
* regras de automação;
* integrações;
* templates;
* configurações gerais.

---

# 8. Dashboard comercial

O módulo deverá possuir um dashboard comercial configurável.

Indicadores sugeridos:

* Leads recebidos.
* Leads qualificados.
* Oportunidades abertas.
* Oportunidades ganhas.
* Oportunidades perdidas.
* Valor total do pipeline.
* Valor previsto.
* Taxa de conversão.
* Ticket médio.
* Ciclo médio de vendas.
* Vendas por vendedor.
* Vendas por equipe.
* Vendas por produto.
* Vendas por origem.
* Vendas por canal.
* Oportunidades paradas.
* Leads sem contato.
* Follow-ups atrasados.
* Atividades realizadas.

O dashboard deverá permitir filtros por:

* período;
* vendedor;
* equipe;
* produto;
* pipeline;
* origem;
* canal;
* etapa.

---

# 9. Pipeline de vendas

O pipeline deverá ser uma das principais interfaces do módulo.

A visualização sugerida será baseada em **Kanban**, permitindo movimentação visual das oportunidades entre etapas.

Exemplo:

```text
NOVO LEAD
    │
    ▼
QUALIFICAÇÃO
    │
    ▼
CONTATO REALIZADO
    │
    ▼
OPORTUNIDADE
    │
    ▼
PROPOSTA
    │
    ▼
NEGOCIAÇÃO
    │
    ├──────────────► PERDIDA
    │
    ▼
FECHAMENTO
    │
    ▼
GANHA
```

Cada oportunidade poderá ser representada por um card contendo informações resumidas.

### Informações sugeridas no card

* Nome do cliente.
* Empresa.
* Valor estimado.
* Produto.
* Vendedor responsável.
* Etapa.
* Origem.
* Última interação.
* Próxima atividade.
* Indicador de atraso.
* Probabilidade de fechamento.
* Data prevista de fechamento.
* Quantidade de dias na etapa.

---

# 10. Pipeline configurável

O sistema deverá permitir múltiplos pipelines.

Exemplos:

### Pipeline de Novos Clientes

```text
Lead
→ Qualificação
→ Diagnóstico
→ Proposta
→ Negociação
→ Fechamento
```

### Pipeline de Telefonia

```text
Lead
→ Qualificação
→ Demonstração
→ Proposta
→ Implantação
→ Venda
```

### Pipeline de Upsell

```text
Cliente Atual
→ Identificação da Oportunidade
→ Contato
→ Proposta
→ Negociação
→ Venda
```

Cada pipeline deverá possuir:

* nome;
* descrição;
* etapas;
* ordem das etapas;
* probabilidade padrão;
* regras;
* campos obrigatórios;
* automações;
* permissões.

---

# 11. Oportunidade

Cada oportunidade deverá possuir uma ficha detalhada.

Informações sugeridas:

* título;
* cliente;
* empresa;
* contato;
* vendedor;
* equipe;
* produto;
* serviço;
* valor;
* moeda;
* origem;
* pipeline;
* etapa;
* probabilidade;
* previsão de fechamento;
* data de criação;
* data da última interação;
* próxima atividade;
* concorrente;
* motivo de perda;
* observações;
* tags;
* campos personalizados.

---

# 12. Timeline unificada

Uma das funcionalidades centrais do módulo deverá ser uma **timeline única de relacionamento**.

A timeline deverá apresentar cronologicamente:

* ligações recebidas;
* ligações realizadas;
* chamadas perdidas;
* gravações;
* mensagens WhatsApp;
* mensagens enviadas;
* mensagens recebidas;
* atividades;
* reuniões;
* alterações de etapa;
* criação de proposta;
* observações;
* tarefas;
* alterações de responsável;
* eventos de automação.

Exemplo:

```text
09:12 — WhatsApp recebido
"Gostaria de saber o valor da solução."

09:15 — Vendedor respondeu

09:21 — Ligação realizada
Duração: 08:42

09:35 — Oportunidade criada
Valor: R$ 3.500

09:40 — Etapa alterada
Qualificação → Proposta

10:00 — Proposta enviada

14:00 — Follow-up agendado
```

O objetivo é que qualquer vendedor autorizado possa abrir o cliente e compreender **toda a história sem precisar procurar informações em diferentes sistemas**.

---

# 13. Inbox omnichannel comercial

O módulo deverá possuir uma caixa de entrada centralizada.

Canais inicialmente considerados:

* WhatsApp;
* telefonia;
* eventualmente SMS;
* e-mail;
* outros canais futuramente.

A interface deverá permitir:

```text
┌──────────────────────────────────────────────┐
│ CONVERSAS                                    │
├──────────────┬───────────────────────────────┤
│ João         │ João — Empresa XPTO           │
│ Maria        │                               │
│ Empresa Y    │ Histórico da conversa         │
│ Carlos       │                               │
│ ...          │                               │
│              │                               │
│              │ [ mensagem ] [ enviar ]       │
└──────────────┴───────────────────────────────┘
```

A conversa deverá apresentar simultaneamente informações comerciais relevantes.

Exemplo:

```text
João Silva
Empresa XPTO

Oportunidade:
Plano Corporativo

Valor:
R$ 4.500/mês

Etapa:
Negociação

Vendedor:
Carlos

Próxima atividade:
Follow-up amanhã 10:00
```

---

# 14. Telefonia integrada

A telefonia deverá ser tratada como parte nativa do processo de vendas.

O vendedor deverá poder:

* realizar chamadas;
* receber chamadas;
* consultar histórico;
* visualizar identificação do contato;
* visualizar oportunidade durante a chamada;
* abrir a ficha do cliente;
* registrar resultado;
* acessar gravação;
* visualizar transcrição quando disponível;
* criar tarefa após a chamada;
* mover a oportunidade de etapa.

---

# 15. Click-to-Call

Números telefônicos exibidos na plataforma deverão permitir chamada direta.

Exemplo:

```text
☎ +55 11 99999-9999
```

Ao clicar, o sistema deverá iniciar a chamada através do mecanismo de telefonia do VIP.

---

# 16. Identificação automática do cliente

Ao receber uma chamada, o sistema deverá tentar localizar automaticamente:

1. telefone;
2. contato;
3. empresa;
4. lead;
5. oportunidade.

Caso exista correspondência, a interface deverá apresentar o contexto comercial.

Caso não exista:

> **Novo contato identificado**

O vendedor poderá criar o cadastro diretamente durante ou após a chamada.

---

# 17. Gravações e transcrições

Quando habilitado, cada chamada poderá possuir:

* gravação;
* duração;
* horário;
* participantes;
* transcrição;
* resumo automático;
* identificação de tópicos;
* sentimento;
* palavras-chave;
* próximos passos.

A transcrição poderá ser utilizada posteriormente por recursos de IA.

---

# 18. WhatsApp

O WhatsApp deverá ser tratado como canal comercial de primeira classe.

O sistema deverá permitir:

* receber mensagens;
* enviar mensagens;
* anexar arquivos;
* enviar imagens;
* enviar documentos;
* utilizar templates;
* identificar contato;
* associar conversa à oportunidade;
* registrar histórico;
* encaminhar conversa;
* transferir atendimento;
* iniciar follow-up;
* automatizar respostas permitidas;
* identificar mensagens não respondidas.

A integração deverá considerar os recursos oficiais disponíveis na plataforma WhatsApp Business/Meta.

---

# 19. Coexistência entre WhatsApp e plataforma

Sempre que tecnicamente possível, deverá ser avaliada a utilização de mecanismos que permitam ao vendedor utilizar o WhatsApp em seus canais autorizados sem perder a sincronização com o CRM.

O objetivo é evitar que o vendedor precise escolher entre:

> "usar o WhatsApp"

ou

> "usar o CRM".

A experiência desejada é:

> **WhatsApp + VIP funcionando como uma única operação comercial.**

---

# 20. Lead Management

O sistema deverá possuir gestão de leads.

Cada lead poderá possuir:

* nome;
* empresa;
* telefone;
* WhatsApp;
* e-mail;
* origem;
* campanha;
* produto de interesse;
* vendedor;
* equipe;
* status;
* temperatura;
* score;
* tags;
* data de entrada;
* última interação;
* próxima atividade.

---

# 21. Origem dos leads

O sistema deverá permitir identificar a origem do lead.

Exemplos:

* WhatsApp;
* telefone;
* site;
* formulário;
* indicação;
* campanha;
* anúncio;
* evento;
* importação;
* API;
* atendimento;
* prospecção ativa;
* cliente existente.

Essa informação deverá ser utilizada posteriormente para análise de ROI e conversão.

---

# 22. Lead Scoring

O sistema poderá possuir um mecanismo de pontuação de leads.

Exemplo:

```text
+10 abriu uma proposta
+20 respondeu WhatsApp
+30 solicitou demonstração
+20 realizou ligação
+10 visitou página comercial
+15 respondeu follow-up

-10 não respondeu
-20 oportunidade parada
-30 informou que não possui interesse
```

Exemplo de classificação:

```text
0–30     Frio
31–60    Morno
61–80    Quente
81–100   Muito quente
```

A metodologia deverá ser configurável.

Futuramente, o score poderá utilizar modelos de IA e comportamento histórico.

---

# 23. Distribuição de leads

O sistema deverá permitir distribuição automática ou manual.

Modelos sugeridos:

### Distribuição sequencial

```text
Carlos
↓
Maria
↓
João
↓
Carlos
```

### Distribuição por carga

O sistema direciona o lead para o vendedor com menor quantidade de oportunidades abertas.

### Distribuição por especialidade

Exemplo:

```text
Telefonia → Equipe A
WhatsApp → Equipe B
Grandes contas → Equipe C
```

### Distribuição geográfica

```text
SP → Equipe SP
PR → Equipe Sul
RJ → Equipe RJ
```

---

# 24. SLA comercial

O sistema poderá permitir definição de SLA para atendimento de leads.

Exemplo:

> Lead recebido → primeiro contato em até 5 minutos.

O sistema deverá sinalizar:

* lead dentro do SLA;
* SLA próximo do vencimento;
* SLA vencido.

Isso permitirá identificar leads que estão sendo desperdiçados por demora no atendimento.

---

# 25. Próxima melhor ação

O sistema deverá ser preparado para sugerir ao vendedor a próxima ação recomendada.

Exemplos:

> "Ligue para João hoje."

> "A proposta está há 3 dias sem resposta."

> "O cliente respondeu ao WhatsApp. Recomenda-se realizar contato."

> "Essa oportunidade está há 7 dias sem interação."

> "O cliente demonstrou interesse em plano corporativo. Recomenda-se enviar proposta."

Inicialmente essas sugestões poderão utilizar regras simples.

Posteriormente poderão utilizar IA.

---

# 26. Follow-up

Cada oportunidade deverá possuir acompanhamento de follow-ups.

O vendedor poderá criar:

* ligação;
* WhatsApp;
* reunião;
* tarefa;
* e-mail;
* demonstração;
* envio de proposta;
* retorno ao cliente.

Cada atividade deverá possuir:

* responsável;
* data;
* hora;
* descrição;
* status;
* prioridade;
* vínculo com oportunidade.

---

# 27. Alertas de oportunidade parada

O sistema deverá identificar oportunidades sem movimentação.

Exemplo:

> ⚠ Oportunidade "Empresa XPTO" está há 8 dias sem interação.

A regra deverá ser configurável por etapa.

Exemplo:

```text
Qualificação: 2 dias
Proposta: 5 dias
Negociação: 7 dias
```

---

# 28. Cadências comerciais

O módulo poderá possuir cadências automáticas.

Exemplo:

```text
Dia 0
WhatsApp

Dia 1
Ligação

Dia 3
WhatsApp

Dia 5
Ligação

Dia 7
Mensagem de encerramento
```

O vendedor poderá acompanhar a cadência e interrompê-la quando houver interação.

---

# 29. Templates

O sistema deverá permitir criação de modelos de comunicação.

Exemplos:

* primeiro contato;
* apresentação;
* follow-up;
* proposta;
* lembrete;
* reativação;
* encerramento;
* pós-venda.

Os templates poderão utilizar variáveis:

```text
Olá {{nome}},

vi que você demonstrou interesse em {{produto}}.

Posso te apresentar nossa solução?
```

---

# 30. Automação comercial

O sistema deverá possuir mecanismo de automação baseado em eventos.

Exemplo:

```text
QUANDO:
Lead criado

SE:
Origem = WhatsApp

ENTÃO:
→ Atribuir à equipe comercial
→ Criar oportunidade
→ Iniciar SLA
→ Criar tarefa
→ Notificar vendedor
```

Outro exemplo:

```text
QUANDO:
Oportunidade entra em "Proposta"

ENTÃO:
→ Criar atividade de follow-up em 2 dias
→ Notificar vendedor
```

---

# 31. IA como copiloto comercial

O módulo deverá ser preparado para utilização de IA.

A IA deverá atuar prioritariamente como **copiloto do vendedor**, e não necessariamente como substituta do vendedor.

Possíveis recursos:

* resumo automático de conversas;
* resumo de chamadas;
* transcrição;
* sugestão de resposta;
* geração de mensagens;
* geração de e-mails;
* identificação de intenção;
* classificação de lead;
* lead scoring;
* sugestão de próxima ação;
* identificação de risco;
* previsão de fechamento;
* análise de oportunidades;
* identificação de objeções;
* extração de informações da conversa;
* atualização automática de campos;
* criação de tarefas a partir da conversa.

A adoção de IA deverá considerar qualidade e confiabilidade dos dados. Pesquisas da Salesforce indicam que apenas uma parcela dos profissionais confia plenamente na precisão dos dados de suas organizações, reforçando que IA deverá ser construída sobre dados estruturados e confiáveis.

---

# 32. Resumo automático da oportunidade

Ao abrir uma oportunidade, o vendedor poderá visualizar um resumo como:

```text
RESUMO DA OPORTUNIDADE

Cliente:
Empresa XPTO

Interesse:
Telefonia corporativa

Valor estimado:
R$ 4.500/mês

Última interação:
Hoje às 09:32

Resumo:
Cliente possui 35 usuários e atualmente utiliza
solução concorrente. Demonstrou interesse em
redução de custos e integração com WhatsApp.

Objeção principal:
Preço.

Próxima ação recomendada:
Realizar contato amanhã e apresentar
proposta comercial revisada.
```

---

# 33. Análise de chamadas por IA

Como diferencial diretamente relacionado à experiência histórica da VIP em voz, o módulo poderá incorporar análise inteligente das chamadas.

Possíveis indicadores:

* duração;
* interrupções;
* tempo de fala;
* tempo de escuta;
* tópicos;
* objeções;
* intenção;
* palavras-chave;
* sentimento;
* menções a concorrentes;
* interesse;
* preço;
* próximos passos.

A ferramenta poderá futuramente identificar automaticamente:

> "Cliente demonstrou interesse, mas apresentou objeção relacionada ao preço."

Esse recurso poderá ser utilizado para treinamento comercial e melhoria de conversão.

---

# 34. Resumo automático de WhatsApp

Da mesma forma, conversas extensas poderão possuir resumo automático.

Exemplo:

```text
Resumo:
Cliente deseja contratar 20 ramais.
Solicitou proposta.
Perguntou sobre portabilidade.
Aguardando retorno do vendedor.
```

---

# 35. Busca global

O sistema deverá possuir busca unificada.

Deverá ser possível buscar por:

* nome;
* telefone;
* WhatsApp;
* empresa;
* e-mail;
* oportunidade;
* código;
* protocolo;
* conteúdo de mensagem;
* conteúdo transcrito;
* tags.

---

# 36. Gestão de empresas

Uma empresa poderá possuir múltiplos contatos.

Exemplo:

```text
EMPRESA XPTO

├── João — Diretor
├── Maria — Financeiro
├── Carlos — TI
└── Pedro — Compras
```

As oportunidades poderão ser associadas à empresa e a um ou mais contatos.

---

# 37. Múltiplos contatos na oportunidade

O sistema deverá permitir que uma oportunidade possua mais de um contato.

Exemplo:

```text
Empresa XPTO
│
├── Decisor
├── Usuário técnico
├── Comprador
└── Financeiro
```

Essa funcionalidade é especialmente importante para vendas B2B.

---

# 38. Produtos e serviços

O módulo deverá permitir cadastrar produtos e serviços comercializados.

Cada produto poderá possuir:

* nome;
* código;
* descrição;
* categoria;
* preço;
* preço mínimo;
* status;
* regras comerciais.

Uma oportunidade poderá conter múltiplos produtos.

---

# 39. Propostas comerciais

Como evolução futura, o módulo poderá permitir criação e acompanhamento de propostas.

Uma proposta poderá possuir:

* cliente;
* oportunidade;
* produtos;
* quantidades;
* valores;
* descontos;
* validade;
* observações;
* responsável;
* status.

Status sugeridos:

```text
Rascunho
Enviada
Visualizada
Em negociação
Aceita
Recusada
Expirada
```

---

# 40. Previsão de vendas

O sistema deverá permitir forecast comercial.

Categorias sugeridas:

* Pipeline;
* Best Case;
* Commit;
* Closed.

Exemplo:

```text
Pipeline:      R$ 500.000
Best Case:     R$ 320.000
Commit:        R$ 220.000
Fechado:       R$ 180.000
```

Futuramente, a previsão poderá utilizar histórico e IA.

---

# 41. Metas

O sistema deverá permitir cadastro de metas.

Exemplos:

* faturamento;
* quantidade de vendas;
* quantidade de oportunidades;
* novos clientes;
* ticket médio;
* conversão.

Metas poderão ser definidas por:

* vendedor;
* equipe;
* período;
* produto;
* unidade.

---

# 42. Relatórios

Relatórios sugeridos:

## Funil

* leads;
* oportunidades;
* propostas;
* vendas;
* perdas.

## Conversão

* conversão por etapa;
* conversão por vendedor;
* conversão por canal;
* conversão por origem.

## Produtividade

* ligações;
* mensagens;
* reuniões;
* atividades;
* follow-ups.

## Receita

* vendas;
* ticket médio;
* MRR/receita recorrente;
* receita por produto;
* receita por vendedor.

## Performance

* ciclo médio;
* tempo por etapa;
* oportunidades paradas;
* SLA.

---

# 43. Análise por canal

O sistema deverá permitir responder:

> Qual canal gera mais vendas?

Exemplo:

```text
Canal             Leads    Vendas    Conversão

WhatsApp           1.200      96       8,0%
Telefonia            500      65      13,0%
Site                 300      24       8,0%
Indicação            100      30      30,0%
```

Essa análise deverá permitir avaliar não apenas volume, mas também qualidade e receita gerada.

---

# 44. Motivos de perda

Toda oportunidade perdida poderá exigir um motivo.

Exemplos:

* preço;
* concorrente;
* sem orçamento;
* timing;
* sem interesse;
* produto inadequado;
* cliente não respondeu;
* contrato existente;
* decisão adiada.

Os motivos deverão ser configuráveis.

---

# 45. Análise de perdas

O sistema deverá permitir identificar padrões.

Exemplo:

```text
35% — Preço
25% — Concorrente
18% — Sem orçamento
12% — Sem retorno
10% — Outros
```

Futuramente, IA poderá analisar conversas associadas às perdas e identificar padrões de objeções.

---

# 46. Gamificação comercial

Como funcionalidade opcional, poderá ser criada uma camada de gamificação.

Exemplos:

* ranking de vendas;
* ranking de conversão;
* número de contatos;
* metas;
* conquistas;
* desafios.

A funcionalidade deverá ser configurável e poderá ser desativada.

---

# 47. Notificações

O sistema poderá enviar notificações sobre:

* novo lead;
* lead atribuído;
* nova mensagem;
* chamada recebida;
* tarefa vencida;
* SLA próximo do vencimento;
* oportunidade parada;
* proposta visualizada;
* oportunidade ganha;
* oportunidade perdida;
* alteração de responsável.

---

# 48. Mobile / responsividade

O módulo deverá possuir interface responsiva.

O vendedor deverá conseguir, preferencialmente pelo dispositivo móvel:

* consultar contatos;
* responder mensagens;
* realizar ligações;
* consultar oportunidades;
* registrar atividades;
* atualizar pipeline;
* visualizar tarefas.

---

# 49. Integrações

O módulo deverá possuir arquitetura preparada para integração com:

* telefonia VIP;
* WhatsApp;
* módulo de atendimento;
* CRM externo;
* APIs;
* ERP;
* sistemas de faturamento;
* plataformas de marketing;
* ferramentas de automação.

As integrações deverão utilizar APIs sempre que possível.

---

# 50. Integração com atendimento

O módulo de vendas deverá conversar diretamente com o módulo de atendimento.

Exemplo:

```text
Cliente entra pelo SAC
        │
        ▼
Atendimento identifica oportunidade
        │
        ▼
"Cliente interessado em novo plano"
        │
        ▼
Criação de oportunidade comercial
        │
        ▼
Equipe de vendas
```

Da mesma forma, vendas poderá encaminhar um cliente para atendimento após a contratação.

---

# 51. Conversão de atendimento em oportunidade

O atendente poderá selecionar:

> **"Criar oportunidade de venda"**

O sistema deverá utilizar os dados já existentes para evitar novo cadastro.

Deverão ser aproveitados, quando disponíveis:

* contato;
* empresa;
* telefone;
* WhatsApp;
* histórico;
* canal;
* assunto;
* atendente;
* conversa.

---

# 52. Permissões

O sistema deverá possuir controle de acesso.

Exemplos:

### Vendedor

Visualiza:

* seus leads;
* suas oportunidades;
* suas atividades.

### Supervisor

Visualiza:

* equipe;
* pipeline da equipe;
* indicadores.

### Gerente

Visualiza:

* toda a operação.

### Administrador

Configura:

* sistema;
* usuários;
* permissões;
* automações.

As permissões deverão ser configuráveis.

---

# 53. Auditoria

Alterações relevantes deverão ser registradas.

Exemplos:

```text
14:32 — Carlos alterou etapa
14:35 — Maria assumiu oportunidade
14:40 — Valor alterado
15:00 — Proposta enviada
```

O histórico deverá registrar:

* usuário;
* data;
* hora;
* ação;
* valor anterior;
* novo valor, quando aplicável.

---

# 54. LGPD e segurança

O módulo deverá considerar requisitos de proteção de dados pessoais.

Deverão ser avaliados:

* controle de acesso;
* princípio do menor privilégio;
* registro de acesso;
* proteção de dados;
* retenção;
* exclusão;
* anonimização quando aplicável;
* gerenciamento de consentimento;
* segurança das integrações;
* proteção das gravações;
* proteção das transcrições.

---

# 55. Segurança de gravações

Gravações de chamadas deverão possuir controle de acesso.

O sistema deverá impedir que qualquer usuário visualize gravações às quais não possui permissão.

Deverá ser avaliada também a utilização de URLs temporárias ou mecanismos equivalentes para acesso aos arquivos.

---

# 56. Requisitos funcionais

## RF-001 — Cadastro de leads

O sistema deverá permitir o cadastro de leads.

## RF-002 — Cadastro de contatos

O sistema deverá permitir cadastrar e editar contatos.

## RF-003 — Cadastro de empresas

O sistema deverá permitir cadastrar empresas e associar contatos.

## RF-004 — Criação de oportunidades

O sistema deverá permitir criar oportunidades vinculadas a leads, contatos ou empresas.

## RF-005 — Pipeline

O sistema deverá permitir visualizar oportunidades em pipeline.

## RF-006 — Movimentação

O sistema deverá permitir mover oportunidades entre etapas.

## RF-007 — Pipelines configuráveis

O sistema deverá permitir criação de múltiplos pipelines.

## RF-008 — Atividades

O sistema deverá permitir criar atividades comerciais.

## RF-009 — Follow-up

O sistema deverá permitir agendamento de follow-ups.

## RF-010 — Histórico

O sistema deverá manter histórico das interações.

## RF-011 — Telefonia

O sistema deverá integrar chamadas telefônicas ao histórico comercial.

## RF-012 — WhatsApp

O sistema deverá integrar mensagens do WhatsApp ao histórico comercial.

## RF-013 — Inbox

O sistema deverá disponibilizar caixa de entrada omnichannel.

## RF-014 — Distribuição

O sistema deverá permitir distribuição de leads.

## RF-015 — SLA

O sistema deverá permitir configurar SLA de primeiro contato.

## RF-016 — Automação

O sistema deverá permitir automações baseadas em eventos.

## RF-017 — Templates

O sistema deverá permitir criação de templates.

## RF-018 — Lead scoring

O sistema deverá permitir classificação de leads por pontuação.

## RF-019 — Relatórios

O sistema deverá disponibilizar relatórios comerciais.

## RF-020 — Dashboard

O sistema deverá disponibilizar dashboard comercial.

## RF-021 — Metas

O sistema deverá permitir gerenciamento de metas.

## RF-022 — Forecast

O sistema deverá permitir previsão de vendas.

## RF-023 — Motivo de perda

O sistema deverá permitir registrar motivo de perda.

## RF-024 — Auditoria

O sistema deverá registrar alterações relevantes.

## RF-025 — Permissões

O sistema deverá possuir controle de acesso por perfil.

## RF-026 — IA

O sistema deverá possuir arquitetura preparada para recursos de Inteligência Artificial.

---

# 57. Requisitos não funcionais

## RNF-001 — Disponibilidade

O módulo deverá possuir disponibilidade compatível com os demais serviços críticos da plataforma VIP.

## RNF-002 — Desempenho

As telas principais deverão apresentar resposta adequada mesmo com elevado volume de oportunidades e interações.

## RNF-003 — Escalabilidade

A arquitetura deverá permitir crescimento de:

* usuários;
* empresas;
* contatos;
* mensagens;
* chamadas;
* oportunidades;
* gravações.

## RNF-004 — Segurança

As informações comerciais deverão ser protegidas contra acesso não autorizado.

## RNF-005 — Auditoria

Operações críticas deverão ser auditáveis.

## RNF-006 — Integração

APIs deverão ser utilizadas sempre que possível para comunicação entre módulos.

## RNF-007 — Responsividade

A interface deverá funcionar adequadamente em diferentes resoluções.

## RNF-008 — Observabilidade

Serviços deverão possuir logs, métricas e mecanismos de monitoramento.

---

# 58. Regras de negócio

## RN-001 — Oportunidade deve possuir responsável

Toda oportunidade ativa deverá possuir um responsável.

## RN-002 — Oportunidade deve possuir etapa

Toda oportunidade deverá estar associada a uma etapa de pipeline.

## RN-003 — Venda ganha

Uma oportunidade somente poderá ser marcada como ganha quando os campos obrigatórios definidos pelo pipeline estiverem preenchidos.

## RN-004 — Venda perdida

O fechamento como perdida poderá exigir o preenchimento de um motivo.

## RN-005 — Follow-up

O sistema deverá permitir identificação de oportunidades sem próxima atividade.

## RN-006 — SLA

Leads deverão ser classificados conforme o cumprimento ou não do SLA configurado.

## RN-007 — Histórico

Alterações relevantes deverão permanecer registradas no histórico.

## RN-008 — Dados duplicados

O sistema deverá tentar evitar duplicidade de contatos utilizando identificadores como telefone, WhatsApp e e-mail.

## RN-009 — Conversão

A conversão de lead para oportunidade deverá preservar o histórico existente.

---

# 59. Critérios de aceitação — exemplos

## CA-001 — Criação de oportunidade

**Dado** um contato existente,

**quando** o vendedor criar uma oportunidade,

**então** o sistema deverá associar automaticamente o contato à oportunidade.

---

## CA-002 — Movimentação no pipeline

**Dado** uma oportunidade ativa,

**quando** o vendedor movimentá-la para outra etapa,

**então** o sistema deverá registrar a alteração no histórico.

---

## CA-003 — Ligação

**Dado** um contato cadastrado,

**quando** o vendedor realizar uma ligação,

**então** a chamada deverá ser registrada no histórico do contato.

---

## CA-004 — WhatsApp

**Dado** uma conversa vinculada ao contato,

**quando** uma nova mensagem for recebida,

**então** a mensagem deverá aparecer no histórico correspondente.

---

## CA-005 — Follow-up

**Dado** uma oportunidade sem atividade futura,

**quando** o vendedor agendar um follow-up,

**então** a atividade deverá ficar vinculada à oportunidade.

---

## CA-006 — Oportunidade parada

**Dado** uma oportunidade que permaneça sem interação pelo período configurado,

**quando** o limite for atingido,

**então** o sistema deverá sinalizar a oportunidade como parada.

---

# 60. Jornada comercial proposta

A jornada ideal poderá seguir o seguinte modelo:

```text
                LEAD
                  │
                  ▼
          IDENTIFICAÇÃO
                  │
                  ▼
            QUALIFICAÇÃO
                  │
          ┌───────┴───────┐
          │               │
       DESCARTE       QUALIFICADO
                          │
                          ▼
                    OPORTUNIDADE
                          │
                          ▼
                      CONTATO
                          │
                          ▼
                      PROPOSTA
                          │
                          ▼
                     NEGOCIAÇÃO
                     ┌────┴────┐
                     │         │
                  PERDIDA    GANHA
                               │
                               ▼
                            CLIENTE
                               │
                               ▼
                         PÓS-VENDA
```

---

# 61. Conceito de "Next Best Action"

Uma evolução importante do produto deverá ser transformar o sistema em um assistente comercial.

Em vez de simplesmente mostrar:

> "Você possui 35 oportunidades."

O VIP deverá futuramente apresentar:

> **"Estas são as 5 oportunidades que você deveria trabalhar agora."**

Exemplo:

```text
PRIORIDADE ALTA

1. Empresa XPTO
   R$ 8.500/mês
   Proposta enviada há 3 dias
   Cliente respondeu ontem
   → Recomenda-se ligação

2. Empresa ABC
   R$ 4.200/mês
   Score 87
   Sem contato há 2 dias
   → Recomenda-se WhatsApp

3. Empresa DEF
   R$ 12.000/mês
   Negociação
   Decisão prevista para esta semana
   → Recomenda-se follow-up
```

Esse conceito deverá ser considerado uma das possíveis diferenciações estratégicas do VIP.

---

# 62. Visão futura — Agente de vendas

Como evolução futura, o VIP poderá possuir agentes de IA capazes de executar tarefas sob regras e autorização.

Exemplos:

* qualificar leads;
* responder perguntas iniciais;
* identificar intenção de compra;
* coletar informações;
* agendar reuniões;
* solicitar dados;
* encaminhar para vendedor;
* realizar follow-ups;
* atualizar CRM;
* resumir conversas;
* sugerir ações.

A atuação deverá ser controlada por regras de negócio e permissões.

O objetivo não deverá ser simplesmente "colocar um chatbot", mas criar uma camada de **automação comercial inteligente**.

---

# 63. Diferencial estratégico de voz

A experiência histórica da VIP em telefonia deverá ser utilizada como diferencial.

Enquanto muitos CRMs tratam a ligação como:

> "Atividade: ligação realizada."

O VIP poderá tratar a ligação como parte integral da jornada comercial.

Exemplo:

```text
Cliente
   │
   ├── WhatsApp
   │
   ├── Ligação
   │      ├── Gravação
   │      ├── Transcrição
   │      ├── Resumo
   │      └── Objeções
   │
   ├── WhatsApp
   │
   └── Proposta
```

Isso possibilita uma plataforma verdadeiramente orientada a **conversas comerciais**, e não apenas a registros de CRM.

---

# 64. Diferencial estratégico do VIP

O posicionamento futuro do módulo poderá ser resumido como:

> **"Do primeiro contato ao fechamento, todas as conversas em um só lugar."**

Ou:

> **"Transforme conversas em oportunidades e oportunidades em vendas."**

A proposta de valor deverá unir:

```text
VOZ
 +
WHATSAPP
 +
ATENDIMENTO
 +
CRM
 +
AUTOMAÇÃO
 +
IA
 =
PLATAFORMA COMERCIAL OMNICHANNEL
```

---

# 65. MVP sugerido

Apesar de este documento apresentar uma visão abrangente, recomenda-se que o primeiro lançamento não tente implementar tudo.

Uma sugestão de MVP:

### Fase 1 — Fundamentos

* Contatos;
* empresas;
* leads;
* oportunidades;
* pipeline;
* etapas;
* vendedores;
* atividades;
* histórico;
* dashboard básico.

### Fase 2 — Omnichannel

* WhatsApp;
* telefonia;
* inbox;
* timeline unificada;
* gravações;
* identificação automática.

### Fase 3 — Automação

* follow-up;
* SLA;
* distribuição de leads;
* templates;
* automações;
* alertas.

### Fase 4 — Inteligência

* transcrição;
* resumo;
* lead scoring;
* próxima melhor ação;
* análise de chamadas;
* IA comercial.

### Fase 5 — Inteligência avançada

* forecast preditivo;
* agentes de IA;
* análise de comportamento;
* recomendação automática;
* automação avançada de cadências.

---

# 66. Itens para validação com a Diretoria Comercial

Esta seção deverá ser utilizada durante a revisão do documento.

## Processo comercial

* [ ] Quais são exatamente as etapas atuais do processo de vendas?
* [ ] Existem diferentes processos por produto?
* [ ] Existem diferentes processos por tipo de cliente?
* [ ] O pipeline deverá ser único ou múltiplo?
* [ ] Quais etapas são obrigatórias?
* [ ] Quais etapas podem ser puladas?

## Leads

* [ ] Quais são as origens atuais dos leads?
* [ ] Como os leads são distribuídos hoje?
* [ ] Existe SLA comercial?
* [ ] Quais critérios definem um lead qualificado?
* [ ] Como a equipe classifica leads quentes/mornos/frios?

## Vendas

* [ ] Quais produtos serão vendidos pelo módulo?
* [ ] Existe venda recorrente?
* [ ] Existe comissão?
* [ ] Existem metas individuais?
* [ ] Existem metas por equipe?
* [ ] Existe necessidade de forecast?

## Comunicação

* [ ] Quais números de WhatsApp serão utilizados?
* [ ] O vendedor poderá utilizar o mesmo número pelo celular?
* [ ] Como as ligações são realizadas atualmente?
* [ ] As gravações deverão ficar disponíveis no CRM?
* [ ] Transcrição será necessária?
* [ ] Quais canais deverão existir no MVP?

## Gestão

* [ ] Quais indicadores a diretoria acompanha atualmente?
* [ ] Quais relatórios são considerados indispensáveis?
* [ ] Quais informações devem estar no dashboard?
* [ ] Quais indicadores são utilizados para cobrança da equipe?

## Automação

* [ ] Quais tarefas são repetitivas atualmente?
* [ ] Quais follow-ups poderiam ser automáticos?
* [ ] Quais eventos devem gerar notificações?
* [ ] Quais atividades devem possuir SLA?

## IA

* [ ] A empresa deseja utilizar IA desde o MVP?
* [ ] Quais informações poderão ser processadas por IA?
* [ ] Transcrição de chamadas será necessária?
* [ ] Resumo automático será necessário?
* [ ] Sugestão de respostas será necessária?
* [ ] Lead scoring automatizado será desejável?
* [ ] A IA poderá executar ações ou apenas recomendar?

---

# 67. Pontos deliberadamente deixados em aberto

Este documento não define definitivamente:

* modelo comercial;
* regras de comissão;
* política de preços;
* estrutura definitiva de pipelines;
* campos obrigatórios;
* critérios definitivos de qualificação;
* regras definitivas de lead scoring;
* regras de distribuição;
* fornecedores de IA;
* fornecedor de WhatsApp;
* arquitetura definitiva;
* política definitiva de retenção de gravações;
* cronograma de desenvolvimento.

Esses itens deverão ser definidos durante as etapas posteriores de análise e desenho do produto.

---

# 68. Princípios de produto

O desenvolvimento do módulo deverá considerar os seguintes princípios:

### 1. O vendedor não deve precisar alimentar o CRM manualmente o tempo inteiro.

Sempre que possível, informações deverão ser capturadas automaticamente a partir das interações.

### 2. Conversas devem gerar dados.

Uma ligação ou mensagem não deverá ser apenas um evento isolado.

### 3. O CRM deve trabalhar para o vendedor.

O sistema deverá sugerir ações, lembrar atividades e identificar riscos.

### 4. Omnichannel significa contexto unificado.

WhatsApp e voz não deverão funcionar como sistemas independentes.

### 5. Dados devem ser reutilizados.

O cliente não deverá precisar repetir informações que o VIP já possui.

### 6. Automação antes de IA.

Processos simples deverão ser resolvidos com regras determinísticas. IA deverá ser utilizada quando agregar inteligência real.

### 7. IA deverá ser baseada no contexto do cliente.

Uma IA comercial sem acesso aos dados corretos será apenas um gerador de texto sofisticado.

### 8. O sistema deverá ser configurável.

Cada operação comercial poderá possuir processos diferentes.

---

# 69. Visão final do produto

A visão de longo prazo para o Módulo de Vendas é transformar o VIP em uma plataforma na qual o vendedor possa executar praticamente toda a sua rotina comercial:

```text
                    ┌──────────────────┐
                    │      LEAD        │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │   QUALIFICAÇÃO   │
                    └────────┬─────────┘
                             │
                             ▼
              ┌─────────────────────────────┐
              │       CONVERSA              │
              │                             │
              │ WhatsApp  ←→  Telefonia    │
              │                             │
              └──────────────┬──────────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │   OPORTUNIDADE   │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │     PIPELINE     │
                    └────────┬─────────┘
                             │
                 ┌───────────┴───────────┐
                 │                       │
                 ▼                       ▼
             AUTOMAÇÃO                 IA
                 │                       │
                 └───────────┬───────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │      VENDA       │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │     CLIENTE      │
                    └──────────────────┘
```

O objetivo final é que o VIP deixe de ser percebido apenas como uma solução de telefonia ou atendimento e passe a ser percebido como uma **plataforma de relacionamento e receita**, na qual comunicação, atendimento e vendas fazem parte da mesma jornada.

---

# 70. Considerações finais

Este documento representa uma **proposta inicial de produto** e deverá ser revisado conjuntamente pelas áreas de Produto, Comercial, Projetos e Tecnologia.

Durante a revisão, cada requisito deverá ser classificado como:

* **Manter**
* **Alterar**
* **Remover**
* **Futuro**
* **Necessita validação**

A versão posterior deverá transformar as decisões comerciais em requisitos funcionais definitivos e, posteriormente, em épicos, histórias de usuário e tarefas de desenvolvimento.

**Versão atual:** 0.1
**Status:** Rascunho
**Próxima etapa:** Validação com a Diretoria Comercial
