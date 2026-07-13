# ROTINAS HUB

## Visão Geral

A aba **ROTINAS HUB** do Node-RED é responsável pela execução automática das rotinas de sincronização entre sistemas externos e o banco de dados do HubVIP.

Todas as tarefas são executadas por meio de scripts **Node.js**, acionados através de nós **Inject** configurados com agendamentos (cron), garantindo que o Data Warehouse permaneça atualizado sem intervenção manual.


# Arquitetura Geral

```
\`\`\`mermaid

flowchart TD  
 A\["04:00 - Importação Clientes"\]  
 B\["04:30 - Importação Tiflux"\]  
 C\["08:00 - Importação Tráfego"\]  
 D\["08:15 - Agregação Tráfego"\]  
 E\["08:20 - Agregação Plano"\]  
 F\["Agendor\<br/\>A cada 30 minutos"\]  
  
 HUB\[(Banco HubVIP)\]  
  
 A --\> HUB  
 B --\> HUB  
 C --\> HUB  
 D --\> HUB  
 E --\> HUB  
 F --\> HUB

\`\`\`
```




# Fluxo 1 - Importação de Clientes

## Objetivo

Atualizar diariamente a base de clientes provenientes das seguintes plataformas:

- IPBCenter

- IPBX

- VipCC

## Agendamento

```
04:00 diariamente
```

## Fluxo

```
`\`\`\`mermaid`
```

```
`flowchart TD`

`    A\[Inject\]`


`    A --\> B\[Importa Clientes IPBCenter\]`

`    A --\> C\[Importa Clientes IPBX\]`

`    A --\> D\[Importa Clientes VipCC\]`


`    B --\> E\[Execução Node.js\]`

`    C --\> E`

`    D --\> E`


`    E --\> F\[Function Validador\]`

`    F --\> G\[Debug\]`

`\`\`\``
```


## Scripts executados

| Script | Finalidade |
| - | - |
| importa\_clientes\_ipbcenter.js | Importa clientes do IPBCenter |
| importa\_clientes\_ipbx.js | Importa clientes do IPBX |
| importa\_clientes\_vipcc.js | Importa clientes do VipCC |


## Nodes utilizados

- Inject

- Exec

- Function

- Debug


# Fluxo 2 - Importação do Tráfego Júpiter

## Objetivo

Importar diariamente os registros de tráfego do sistema Júpiter para atualização do banco de dados.

## Agendamento

```
08:00 diariamente
```

## Scripts executados

| Script | Finalidade |
| - | - |
| importa\_trafego\_jupiter\_licenciados.js | Importa tráfego dos clientes licenciados |
| importa\_trafego\_jupiter\_geral.js | Importa tráfego geral |



# Fluxo 3 - Agregação do Tráfego

## Objetivo

Consolidar os dados importados do Júpiter para utilização em dashboards e consultas do Metabase.

## Agendamento

```
08:15 diariamente
```

## Script executado

```
agrega\_trafego\_jupiter.js
```

### Resultado

Gera tabelas agregadas para consultas analíticas e relatórios.


# Fluxo 4 - Agregação Plano Full

## Objetivo

Executar a agregação específica dos dados referentes ao Plano Full.

## Agendamento

```
08:20 diariamente
```

## Script executado

```
agrega\_plano\_full.js
```


# Fluxo 5 - Importação Tiflux

## Objetivo

Processar diariamente o arquivo TSV exportado pelo Tiflux e armazenar seus dados no banco de dados.

## Agendamento

```
04:30 diariamente
```

## Script executado

```
import\_tiflux\_tickets.js
```

## Etapas do processamento

1. Leitura do arquivo TSV.

2. Tratamento dos registros.

3. Inserção dos dados no banco.

4. Atualização das tabelas utilizadas pelo HubVIP.


# Fluxo 6 - Integração Agendor

## Objetivo

Manter sincronizadas as informações do CRM Agendor com o banco do HubVIP.

## Frequência

```
A cada 30 minutos
```

## Scripts executados

| Script | Finalidade |
| - | - |
| import\_agendor\_atividades.js | Importa atividades |
| import\_agendor\_usuarios.js | Importa usuários |
| import\_agendor\_pessoas.js | Importa pessoas |
| import\_agendor\_empresas.js | Importa empresas |
| import\_agendor\_negocios.js | Importa negócios |



# Componentes Utilizados

## Inject

O node **Inject** é responsável por iniciar automaticamente uma rotina através de um agendamento configurado em formato Cron.

Exemplo:

```
00 04 \* \* \*
```

Executa diariamente às **04:00**.


## Exec

O node **Exec** executa processos externos ao Node-RED.

Neste projeto, ele é utilizado para executar scripts Node.js.

Exemplo:

```
node /opt/scripts/js/import\_agendor\_negocios.js
```


## Function

O node **Function** executa pequenos trechos de código JavaScript.

Neste fluxo é utilizado para liberar a execução de novas rotinas após a conclusão do script.

Código utilizado:

```
flow.set('running', false);  
return msg;
```


## Debug

O node **Debug** exibe mensagens de saída no painel do Node-RED.

É utilizado para:

- acompanhar a execução dos scripts;

- identificar erros;

- validar retornos.


# Ordem de Execução

| Horário | Processo |
| - | - |
| 04:00 | Importação de Clientes |
| 04:30 | Importação Tiflux |
| 08:00 | Importação Tráfego Júpiter |
| 08:15 | Agregação de Tráfego |
| 08:20 | Agregação Plano Full |
| A cada 30 minutos | Integração Agendor |



# Dependências

```
flowchart LR  
  
Clientes --\> Banco  
  
Tiflux --\> Banco  
  
Tráfego --\> Banco  
  
Banco --\> Agregação  
  
Agregação --\> Metabase
```


# Estrutura dos Scripts

Todos os scripts executados pelo Node-RED encontram-se em:

```
/opt/scripts/js
```

## Exemplos

| Script | Tecnologia | Local |
| - | - | - |
| importa\_clientes\_ipbx.js | Node.js | /opt/scripts/js |
| agrega\_trafego\_jupiter.js | Node.js | /opt/scripts/js |
| import\_agendor\_negocios.js | Node.js | /opt/scripts/js |



# Considerações Finais

A aba **ROTINAS HUB** concentra as principais rotinas automáticas responsáveis pela atualização do banco de dados do HubVIP.

Seu funcionamento é baseado em:

- agendamentos automáticos;

- execução de scripts Node.js;

- sincronização com sistemas externos;

- atualização das tabelas utilizadas pelo Metabase;

- monitoramento através dos nodes Debug.

Essa arquitetura permite manter as informações do ambiente sempre atualizadas, reduzindo intervenções manuais e centralizando toda a automação operacional em um único fluxo do Node-RED.

