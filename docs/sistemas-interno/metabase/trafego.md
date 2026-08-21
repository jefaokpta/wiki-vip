# Manual — Relatórios de Tráfego e DETRAF

## 1. Objetivo

Este documento apresenta os principais relatórios de **tráfego de telefonia da VIP**, disponíveis nos dashboards de acompanhamento operacional e financeiro.

Os relatórios permitem acompanhar:

- Volume de chamadas realizadas e recebidas;
- Minutos trafegados;
- Distribuição do tráfego entre operadoras;
- Valores tarifados;
- Evolução mensal e trimestral do tráfego;
- Identificação de chamadas que não puderam ser tarifadas;
- Distribuição do tráfego por tronco;
- Tráfego de licenciados;
- Informações relacionadas ao STFC e ao Summit.

O objetivo é fornecer uma visão operacional e financeira do tráfego de voz, permitindo identificar alterações de comportamento, inconsistências de tarifação e concentração de tráfego em determinadas operadoras.

> **Importante:** os gráficos possuem finalidades diferentes. Alguns são voltados ao acompanhamento financeiro e ao DETRAF, enquanto outros possuem finalidade predominantemente operacional e estatística.

---

# 2. Conceitos importantes

Antes de utilizar os relatórios, é importante diferenciar alguns conceitos utilizados nos dashboards.

### Tráfego

Representa o volume de chamadas processadas pela infraestrutura de telefonia.

Dependendo do relatório, o tráfego pode ser apresentado em:

- Quantidade de chamadas;
- Minutos;
- Valores financeiros;
- Operadora;
- Tronco;
- Direção da chamada (entrada ou saída).

### Minutos trafegados

Representam a duração acumulada das chamadas dentro do período analisado.

Exemplo:

> 100 chamadas com duração média de 3 minutos representam aproximadamente 300 minutos de tráfego.

### Operadora

É a operadora de telefonia associada à origem ou ao destino da chamada, dependendo do relatório.

Nos relatórios do **Jupiter Billing**, as principais operadoras normalmente observadas são:

- Agera;
- New Voice;
- Pentágono;
- TelGlobe.

Nos relatórios de **STFC Summit**, as principais operadoras normalmente observadas são:

- Telefônica;
- Claro;
- Vivo;
- TIM.

> A lista de operadoras não é necessariamente limitada a essas empresas. Elas representam as principais operadoras normalmente encontradas nos relatórios.

### Tronco

Representa o caminho ou grupo de recursos de telefonia utilizado para transportar o tráfego.

A análise por tronco permite identificar como o tráfego está sendo distribuído dentro da infraestrutura da VIP, independentemente de uma análise exclusivamente por operadora.

---

# 3. Tráfego Jupiter Billing

## 3.1 Visão geral

A seção **Tráfego Jupiter Billing** concentra os dados utilizados para acompanhamento do tráfego tarifado e dos valores relacionados ao **DETRAF das operadoras**.
O sistema de DETRAF utiliza as tarifas cadastradas no sistema de tarifação desenvolvido pelo Salomão. Nesse sistema são mantidas as informações necessárias para determinar o valor de cada chamada, incluindo:

- Operadora;
- Tarifa;
- Tipo de chamada;
- Centro de custo;
- Regras de tarifação;
- Demais parâmetros necessários para o cálculo.

Dessa forma, os valores apresentados nos relatórios dependem diretamente da qualidade e da atualização dos cadastros de tarifação.

### Principais utilizações

Essa seção é utilizada principalmente para:

1. Acompanhar o valor financeiro do tráfego;
2. Comparar o volume de minutos entre operadoras;
3. Identificar chamadas que não foram tarifadas;
4. Apoiar o fechamento do DETRAF;
5. Analisar a evolução do tráfego ao longo dos meses;
6. Identificar concentração de tráfego em determinadas operadoras ou troncos.

---

# 4. Gráficos principais — Jupiter Billing

A visão principal possui quatro relatórios.

---

## 4.1 Valor Total Semanal

**Tipo:** Gráfico de pizza  
**Métrica:** Valor financeiro  
**Segmentação:** Operadora  
**Período:** Semanal

Este gráfico apresenta a **soma do valor total tarifado no período semanal**, distribuída entre as operadoras.

Cada segmento da pizza representa a participação de uma operadora no valor total do tráfego.
### Exemplo de interpretação

Imagine que o relatório apresente:

| Operadora | Valor |
|---|---:|
| Agera | R$ 12.000 |
| New Voice | R$ 8.000 |
| Pentágono | R$ 5.000 |
| TelGlobe | R$ 3.000 |

O gráfico permitirá visualizar rapidamente que a **Agera representa a maior parcela financeira do tráfego no período**.

### Para que serve?

Esse relatório é especialmente útil para:

- Acompanhar o impacto financeiro do tráfego;
- Identificar quais operadoras concentram maior valor;
- Comparar alterações semanais;
- Auxiliar no acompanhamento do DETRAF;
- Identificar mudanças significativas no perfil de utilização.

> **Atenção:** uma operadora possuir maior participação em valor não significa necessariamente que possua maior quantidade de minutos. O valor depende das tarifas aplicadas às chamadas.

---

## 4.2 Minutos por Operadora — Semanal

**Tipo:** Gráfico de pizza  
**Métrica:** Minutos trafegados  
**Segmentação:** Operadora  
**Período:** Semanal

Apresenta a **quantidade total de minutos trafegados**, distribuída por operadora.

Enquanto o gráfico anterior responde:

> "Qual operadora representa maior valor financeiro?"

este gráfico responde:

> "Para qual operadora estamos enviando maior volume de minutos?"

Essa distinção é importante porque diferentes operadoras podem possuir tarifas diferentes.

### Exemplo

Uma operadora pode representar:
- 40% dos minutos;
- mas apenas 25% do valor.

Outra pode representar:

- 20% dos minutos;
- mas 35% do valor.

Isso pode ocorrer devido às diferenças entre as tarifas aplicadas.

### Para que serve?

É utilizado para:

- Acompanhar o volume de utilização;
- Identificar concentração de tráfego;
- Comparar volume versus custo;
- Identificar mudanças no comportamento do tráfego;
- Auxiliar na análise de custos das operadoras.

---

## 4.3 Billing Erros

**Tipo:** Tabela  
**Finalidade:** Identificação de chamadas não tarifadas

A tabela **Billing Erros** apresenta chamadas para as quais o sistema não conseguiu realizar o cálculo da tarifa.

Esses registros devem ser analisados principalmente porque uma chamada sem tarifação pode representar um valor financeiro que deixou de ser calculado.

### Principal causa

Na maioria dos casos, o problema está relacionado à **ausência de uma tarifa correspondente no cadastro do sistema de tarifação**.

Entretanto, outros problemas também podem causar falhas, como:

- Dados incompletos da chamada;
- Operadora não identificada;
- Tipo de chamada não cadastrado;
- Centro de custo não configurado;
- Inconsistências nos dados de origem;
- Alterações de operadora ou de regras de tarifação.

### Procedimento recomendado

Quando forem identificados registros nessa tabela:

1. Verificar a operadora da chamada;
2. Verificar o tipo de chamada;
3. Verificar o destino;
4. Verificar o tronco utilizado;
5. Conferir os dados da chamada;
6. Verificar se existe tarifa correspondente no sistema de tarifação;
7. Corrigir o cadastro, quando necessário;
8. Reprocessar a tarifação, quando aplicável.

> **Boa prática:** o ideal é que o relatório **Billing Erros permaneça sem registros** após o processamento completo do tráfego. A existência de erros deve ser investigada antes do fechamento do DETRAF.

---

## 4.4 Tráfego Trimestral por Operadora

**Tipo:** Gráfico de barras  
**Período:** Trimestre  
**Agrupamento:** Mês → Operadora  
**Métrica:** Valor financeiro

Este relatório apresenta a **soma mensal dos valores tarifados dentro de um trimestre**, permitindo comparar simultaneamente os meses e as operadoras.

O gráfico possui duas dimensões principais:

- Mês;
- Operadora.

### Exemplo conceitual

Um trimestre pode apresentar:

Julho
  Agera       ███████████
  New Voice   ███████
  Pentágono   ████
  TelGlobe    ██

Agosto
  Agera       █████████
  New Voice   ████████
  Pentágono   █████
  TelGlobe    ███

Setembro
  Agera       ████████████
  New Voice   ██████
  Pentágono   ███
  TelGlobe    ███

### Principal utilização

Este é um dos relatórios mais importantes para o acompanhamento do **DETRAF**, pois permite visualizar:

- Evolução mensal do valor;
- Participação de cada operadora;
- Crescimento ou redução do tráfego;
- Mudanças na distribuição financeira;
- Diferenças entre os meses do trimestre.

---

# 5. Aba Avançado — Jupiter Billing

A aba **Avançado** apresenta análises complementares relacionadas à distribuição do tráfego por tronco.

Ela é destinada principalmente a análises mais detalhadas da infraestrutura de saída.

---

## 5.1 Tráfego Somado — BINA Inteligente — 3 Meses

**Tipo:** Gráfico de pizza  
**Período:** Últimos 3 meses  
**Segmentação:** Tronco  
**Métrica:** Tráfego

Este gráfico apresenta a soma do tráfego realizado através das diferentes saídas utilizadas pelo mecanismo de **BINA Inteligente** durante o período de três meses.

A distribuição é feita por tronco, permitindo identificar quais caminhos de saída estão sendo mais utilizados.

### Exemplo de distribuição

O exemplo abaixo utiliza **valores ilustrativos**, apenas para demonstrar como a distribuição do tráfego pode ser apresentada:

~~~mermaid
pie title Tráfego por Tronco — Últimos 3 Meses
    "Tronco A" : 45
    "Tronco B" : 30
    "Tronco C" : 15
    "Tronco D" : 10
~~~

Nesse exemplo:

- **Tronco A:** 45% do tráfego;
- **Tronco B:** 30% do tráfego;
- **Tronco C:** 15% do tráfego;
- **Tronco D:** 10% do tráfego.

O gráfico permite visualizar rapidamente a participação de cada tronco no tráfego total.

### Para que serve?

É útil para:

- Avaliar a distribuição das chamadas;
- Identificar concentração em determinados troncos;
- Acompanhar o comportamento das rotas;
- Identificar mudanças relevantes na utilização das saídas;
- Apoiar análises de engenharia de tráfego.

### Exemplo

Se um único tronco representar uma parcela muito elevada do tráfego, isso pode indicar uma forte concentração da utilização naquela rota.

---

## 5.2 Tráfego Somado — Jupiter Geral por Tronco — 3 Meses

**Tipo:** Gráfico de pizza  
**Período:** Últimos 3 meses  
**Segmentação:** Tipo de tronco / Operadora  
**Métrica:** Tráfego

Este relatório apresenta a soma geral das saídas realizadas pelo Jupiter durante os três meses anteriores, permitindo analisar a participação dos diferentes troncos e operadoras.

Diferentemente do relatório específico de BINA Inteligente, este gráfico possui uma visão mais ampla do tráfego do Jupiter.

### Exemplo de distribuição

O exemplo abaixo utiliza valores ilustrativos para representar a distribuição do tráfego entre os diferentes troncos:

~~~mermaid
pie title Tráfego Jupiter Geral por Tronco — Últimos 3 Meses
    "Agera" : 35
    "New Voice" : 30
    "Pentágono" : 20
    "TelGlobe" : 15
~~~

### Para que serve?

É utilizado para:

- Analisar a distribuição geral do tráfego;
- Identificar os troncos mais utilizados;
- Comparar a participação das operadoras;
- Identificar concentração de tráfego;
- Apoiar decisões relacionadas a roteamento e capacidade.

---

# 6. Tráfego STFC Summit

## 6.1 Visão geral

A seção **Tráfego STFC Summit** apresenta informações relacionadas ao tráfego de telefonia do **STFC da VIP**, com foco nas operadoras que mais participam do tráfego dentro da rede.

Os relatórios permitem analisar tanto:

- **Chamadas de saída**, observando a operadora de destino;
- **Chamadas de entrada**, observando a operadora de origem.

As principais operadoras normalmente observadas nessa seção são:

- Telefônica;
- Claro;
- Vivo;
- TIM.

A análise do STFC é especialmente útil para acompanhamento do comportamento do tráfego, composição das operadoras e evolução do volume de chamadas.

---

# 7. Relatórios do STFC Summit

## 7.1 Ligações de Saída x Operadora Destino

**Tipo:** Gráfico de pizza  
**Período:** Últimos 30 dias, incluindo o dia atual  
**Métrica:** Minutos  
**Segmentação:** Operadora de destino

Apresenta a distribuição dos minutos das **ligações de saída**, agrupados de acordo com a operadora para a qual as chamadas foram direcionadas.

### Exemplo de interpretação

Se o gráfico apresentar:

- Vivo — 35%;
- Claro — 30%;
- TIM — 20%;
- Telefônica — 15%;

isso significa que, dentro do período analisado, a maior parte dos minutos de chamadas de saída teve como destino a Vivo.

### Exemplo visual

~~~mermaid
pie title Ligações de Saída x Operadora Destino
    "Vivo" : 35
    "Claro" : 30
    "TIM" : 20
    "Telefônica" : 15
~~~

### Para que serve?

Permite:

- Identificar as operadoras que mais recebem chamadas da VIP;
- Acompanhar a concentração do tráfego;
- Identificar mudanças no perfil de utilização;
- Comparar o volume entre operadoras.

---

## 7.2 Ligações de Entrada x Operadora de Origem

**Tipo:** Gráfico de pizza  
**Período:** Últimos 30 dias, incluindo o dia atual  
**Métrica:** Minutos  
**Segmentação:** Operadora de origem

Apresenta a distribuição dos minutos das **ligações recebidas pela VIP**, agrupados de acordo com a operadora de onde as chamadas foram originadas.

Enquanto o relatório anterior analisa:

> **VIP → Operadora de destino**

este relatório analisa:

> **Operadora de origem → VIP**

Essa diferença é importante para entender o fluxo de tráfego em ambas as direções.

---

## 7.3 Tráfego de Saída com Cadência

**Tipo:** Gráfico de pizza  
**Período:** Mês anterior fechado  
**Métrica:** Minutos  
**Segmentação:** Operadora  
**Tratamento:** Cadenciado

Este relatório apresenta o tráfego de saída do **mês anterior já encerrado**, agrupado por operadora e considerando a cadência aplicável ao tráfego.

A utilização do mês anterior fechado evita que o resultado seja alterado diariamente durante o período de análise.

### Para que serve?

É indicado para:

- Análise consolidada do tráfego;
- Comparação entre operadoras;
- Avaliação do volume efetivamente trafegado no período;
- Apoio a análises financeiras e operacionais;
- Comparação com períodos anteriores.

> **Importante:** por utilizar o mês anterior fechado, este relatório não deve ser interpretado como uma visão em tempo real.

---

## 7.4 Tráfego de Entrada em Minutos

**Tipo:** Gráfico de pizza  
**Período:** Mês anterior fechado  
**Métrica:** Minutos  
**Segmentação:** Operadora de origem

Apresenta a soma dos minutos das **ligações recebidas pela VIP durante o mês anterior fechado**, agrupadas pela operadora responsável pela origem da chamada.

O relatório permite entender quais operadoras estão gerando maior volume de tráfego de entrada para a rede.

### Para que serve?

É utilizado para:

- Acompanhar o volume de chamadas recebidas;
- Identificar as principais operadoras de origem;
- Comparar o tráfego de entrada entre períodos;
- Avaliar a composição do tráfego recebido.

---

## 7.5 Saída de Ligações por Operadora Destino

**Tipo:** Tabela  
**Período:** Mês anterior fechado  
**Métrica:** Minutos  
**Segmentação:** Operadora de destino

Esta tabela apresenta uma visão detalhada das ligações de saída realizadas durante o mês anterior, agrupando os dados de acordo com a operadora de destino.

Por ser uma tabela, permite uma análise mais detalhada do que os gráficos de pizza.

### Para que serve?

É utilizada principalmente para:

- Conferir os minutos trafegados;
- Identificar as principais operadoras de destino;
- Apoiar análises detalhadas;
- Comparar informações apresentadas nos gráficos;
- Realizar conferências antes de análises ou fechamentos.

---

# 8. Tráfego Licenciados Jupiter

## 8.1 Visão geral

A seção **Tráfego Licenciados Jupiter** apresenta exclusivamente o tráfego relacionado aos licenciados do Jupiter.

Essa área possui uma finalidade predominantemente **operacional e de acompanhamento**.

Não são aplicados filtros avançados ou análises específicas de DETRAF nessa seção.

O objetivo principal é permitir uma visão simples do volume de tráfego gerado pelos licenciados e acompanhar o comportamento desse tráfego ao longo do tempo.

### Utilização

O relatório pode ser utilizado para:

- Acompanhar o tráfego de licenciados;
- Observar variações de volume;
- Identificar movimentações fora do padrão;
- Apoiar o acompanhamento operacional.

> **Importante:** esta seção não deve ser utilizada como principal fonte para análises de STFC, DETRAF ou controle financeiro do tráfego. Para essas finalidades, devem ser utilizados os relatórios específicos de **Jupiter Billing** e **STFC Summit**.

---

# 9. Como utilizar os relatórios em conjunto

Os relatórios tornam-se mais úteis quando analisados em conjunto.

Uma análise básica pode seguir a seguinte sequência:

### 1. Verificar o volume de minutos

Comece pelos gráficos de minutos para entender **quanto tráfego foi realizado** e como ele está distribuído entre as operadoras.

### 2. Verificar o valor financeiro

Em seguida, compare o volume de minutos com o **valor tarifado**.

Uma alteração significativa no valor sem alteração proporcional nos minutos pode indicar mudança na composição do tráfego ou nas tarifas aplicadas.

### 3. Verificar os Billing Erros

Antes de utilizar os valores para fechamento, verifique a tabela **Billing Erros**.

Chamadas não tarifadas podem fazer com que o valor apresentado não represente todo o tráfego processado.

### 4. Analisar a evolução mensal

Utilize o gráfico **Tráfego Trimestral por Operadora** para identificar tendências.

Procure por:

- Crescimento;
- Redução;
- Mudança de operadora predominante;
- Picos de utilização;
- Alterações repentinas no valor financeiro.

### 5. Analisar os troncos

Caso seja necessário entender **como o tráfego está sendo distribuído tecnicamente**, utilize os relatórios da aba **Avançado**.

Eles permitem sair de uma visão exclusivamente financeira e observar a distribuição por tronco.

### 6. Comparar entrada e saída

No STFC Summit, compare:

- Tráfego de saída por operadora de destino;
- Tráfego de entrada por operadora de origem.

Essa comparação ajuda a entender o comportamento geral do tráfego da rede.

---

# 10. Pontos de atenção

Durante a análise dos dashboards, alguns comportamentos merecem atenção especial.

## 10.1 Aumento de valor sem aumento proporcional de minutos

Pode indicar:

- Alteração na composição do tráfego;
- Maior utilização de destinos com tarifas mais elevadas;
- Alteração de tarifas;
- Mudança de operadora utilizada.

---

## 10.2 Aumento de minutos sem aumento proporcional do valor

Pode ocorrer quando:

- O tráfego passa a utilizar rotas mais econômicas;
- Há maior participação de operadoras com tarifas menores;
- O perfil dos destinos mudou.

---

## 10.3 Aumento de Billing Errors

O crescimento de registros na tabela **Billing Erros** deve ser investigado.

Uma das primeiras verificações deve ser a existência de tarifas cadastradas para a combinação de operadora, tipo de chamada e demais parâmetros utilizados pela tarifação.

---

## 10.4 Concentração excessiva em uma operadora ou tronco

Uma concentração muito elevada pode ser normal dependendo da configuração da rede e das rotas disponíveis.

Entretanto, alterações bruscas na distribuição devem ser investigadas, especialmente quando não existe uma justificativa operacional conhecida.

---

## 10.5 Diferença entre período aberto e período fechado

É importante observar o período considerado pelo relatório.

### Período móvel

Exemplo:

> Últimos 30 dias, contando hoje.

O resultado pode mudar diariamente.

### Período fechado

Exemplo:

> Mês anterior fechado.

O resultado representa um período já encerrado e, portanto, é mais adequado para comparações consolidadas e análises de fechamento.

---

# 11. Resumo dos relatórios

| Seção | Relatório | Período | Principal finalidade |
|---|---|---|---|
| Jupiter Billing | Valor Total Semanal | Semanal | Acompanhar valor financeiro por operadora |
| Jupiter Billing | Minutos por Operadora | Semanal | Acompanhar volume de minutos |
| Jupiter Billing | Billing Erros | Conforme dados processados | Identificar chamadas não tarifadas |
| Jupiter Billing | Tráfego Trimestral por Operadora | Trimestral | Analisar evolução financeira mensal |
| Jupiter Avançado | BINA Inteligente | 3 meses | Analisar distribuição por tronco |
| Jupiter Avançado | Jupiter Geral por Tronco | 3 meses | Analisar distribuição geral das saídas |
| STFC Summit | Saída x Operadora Destino | Últimos 30 dias | Analisar tráfego de saída |
| STFC Summit | Entrada x Operadora Origem | Últimos 30 dias | Analisar tráfego de entrada |
| STFC Summit | Saída com Cadência | Mês anterior fechado | Analisar tráfego de saída consolidado |
| STFC Summit | Entrada em Minutos | Mês anterior fechado | Analisar tráfego de entrada consolidado |
| STFC Summit | Saída por Operadora Destino | Mês anterior fechado | Conferência detalhada do tráfego |
| Licenciados Jupiter | Tráfego de Licenciados | Conforme dashboard | Acompanhamento operacional |

---

# 12. Considerações finais

Os dashboards de tráfego foram desenvolvidos para oferecer diferentes níveis de visão sobre a utilização da infraestrutura de telefonia da VIP.

De maneira geral:

- **Jupiter Billing** deve ser utilizado principalmente para acompanhamento de **tarifação, valores e DETRAF**;
- **Jupiter Avançado** deve ser utilizado para análises mais detalhadas de **rotas e troncos**;
- **STFC Summit** deve ser utilizado para acompanhamento do **tráfego de entrada e saída do STFC**, principalmente por operadora;
- **Licenciados Jupiter** deve ser utilizado para o **acompanhamento operacional do tráfego de licenciados**.

Nenhum gráfico deve ser analisado isoladamente quando o objetivo for investigar uma alteração relevante. A combinação entre **minutos, valores, operadoras, período, erros de tarifação e distribuição por tronco** fornece uma visão muito mais confiável do comportamento do tráfego.

Em especial para análises de DETRAF, recomenda-se sempre verificar se existem **Billing Erros** antes de considerar os valores apresentados como definitivos.
