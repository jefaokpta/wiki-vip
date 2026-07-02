# **DOCUMENTO DE ESTRUTURAÇÃO DE PROJETO – SISTEMA WEB DE GESTÃO DE CHAMADOS**

## **1. IDENTIFICAÇÃO DO PROJETO**

**Nome do Projeto: Vip Tickets**

**Versão do Documento: 1.0**

**Data: 30/06/2026**

**Patrocinador: Vip Solutions**

**Gerente do Projeto: Caio**

**Equipe Envolvida: Salomão, Caio**

**Stakeholders: Gabriela, Andreza, Isabela, Thailym**


## **2. VISÃO GERAL**

### **2.1 Objetivo**

Desenvolver um sistema web para gerenciamento de chamados técnicos, atendimento ao cliente e acompanhamento de demandas operacionais, permitindo o registro, categorização, distribuição, acompanhamento e resolução de solicitações.

### **2.2 Problema Atual**

- Controle manual de chamados.

- Falta de rastreabilidade.

- Dificuldade de medir SLA.

- Informações descentralizadas.

- Baixa visibilidade da produtividade da equipe.

- Ausência de indicadores gerenciais.

### **2.3 Objetivos de Negócio**

- Centralizar o atendimento.

- Reduzir o tempo médio de resolução.

- Melhorar a experiência do cliente.

- Aumentar a produtividade operacional.

- Gerar indicadores estratégicos. (Hub)

### **2.4 Indicadores de Sucesso (KPIs)**

- Tempo médio de primeira resposta.

- Tempo médio de resolução.

- Percentual de cumprimento de SLA.

- Taxa de reabertura de chamados.

- Nível de satisfação do cliente.

- Quantidade de chamados por categoria. (Hub)


## **3. ESCOPO DO PROJETO**

### **3.1 Escopo Incluído**

- Cadastro de usuários.

- Abertura de chamados.

- Gestão de filas. (Facultativo)

- Gestão de SLA.

- Permitir alterar SLA minímo de reabertura do ticket

- Painéis gerenciais.

- Histórico de atendimentos. (Facultativo)

- Base de conhecimento. 

- Notificações automáticas.

- Relatórios operacionais. (Hub)

### **3.2 Escopo Excluído**

- Atendimento telefônico integrado.

- Desenvolvimento de aplicativo móvel.

- Integrações não previstas.

- Gestão financeira.


## **4. PERFIS DE USUÁRIO**

### **Cliente**

Responsável por abrir e acompanhar chamados.

### **Atendente**

Responsável pelo atendimento e resolução.

### **Supervisor**

Responsável pela gestão operacional.

### **Administrador**

Responsável pela parametrização do sistema.


## **5. LEVANTAMENTO DE REQUISITOS**

### **5.1 Requisitos Funcionais**

| **Código** | **Requisito** | **Prioridade** |
| :-: | :-: | :-: |
| RF-001 | Permitir cadastro de usuários | Alta |
| RF-002 | Permitir abertura de chamados | Alta |
| RF-003 | Permitir anexar arquivos | Alta |
| RF-004 | Permitir categorização de chamados | Alta |
| RF-005 | Permitir atribuição automática de atendentes | Média |
| RF-006 | Controlar SLA | Alta |
| RF-007 | Registrar histórico de interações | Alta |
| RF-008 | Permitir pesquisa de chamados | Alta |
| RF-009 | Gerar relatórios | Média |
| RF-010 | Disponibilizar dashboards | Média |
| RF-011 | Enviar notificações automáticas | Alta |
| RF-012 | Disponibilizar portal do cliente | Alta |
| RF-013 | Permitir criação de base de conhecimento | Média |

### **5.2 Requisitos Não Funcionais**

| **Código** | **Requisito** |
| :-: | :-: |
| RNF-001 | Sistema responsivo |
| RNF-002 | Disponibilidade mínima de 99,5% |
| RNF-003 | Autenticação segura |
| RNF-004 | Criptografia de dados sensíveis |
| RNF-005 | Registro de auditoria |
| RNF-006 | Tempo de resposta inferior a 3 segundos |
| RNF-007 | Compatibilidade com principais navegadores |
| RNF-008 | Escalabilidade horizontal |
| RNF-009 | Conformidade com LGPD (Verificar se é necessário) |


## **6. REGRAS DE NEGÓCIO**

- Todo chamado deve possuir um solicitante.

- Todo chamado deve possuir categoria e prioridade.

- Chamados encerrados não podem ser alterados sem reabertura.

- O SLA deve considerar horário operacional.

- O cliente pode avaliar o atendimento após o encerramento.


## **7. CLASSIFICAÇÃO DOS CHAMADOS**

### **Categorias**

- Incidente

- Requisição

- Dúvida

- Melhoria

- Problema

### **Prioridades**

- Baixa

- Média

- Alta

- Crítica

### **Status**

- Aberto

- Em atendimento

- Aguardando cliente

- Escalado

- Resolvido

- Encerrado


## **8. INTEGRAÇÕES**

### **Obrigatórias**

- E-mail (API ou SMTP)

- Bucket S3 (Ver Tempo de Armazenametno)

- API REST (Autenticado)

- WhatsApp (Chat de Comunicação)

- Chat via Web

### **Desejáveis**

- Microsoft Teams

- Slack

- ERP

- CRM

- Telefonia (Vip ou outros fornecedores de PABX/VoIP)

### **Requisitos das Integrações**

- Método de autenticação.

- Frequência de sincronização.

- Dados trafegados.

- Responsável técnico.


## **9. ARQUITETURA DA SOLUÇÃO**

### **Front-end**

- Tecnologias previstas:

### **Back-end**

- Tecnologias previstas:

### **Banco de Dados**

- Tecnologia prevista:

### **Hospedagem**

- Cloud ou On-premises:

### **Infraestrutura**

- Docker

- Versionamento no GitHub

- Backup automático

- Monitoramento


## **10. SEGURANÇA E CONFORMIDADE**

- Controle de acesso por perfil.

- Registro de logs.

- Auditoria de ações.

- Política de senhas.

- Autenticação multifator.

- Gestão de permissões.

- Adequação à LGPD.


## **11. EXPERIÊNCIA DO USUÁRIO**

### **Telas Previstas**

- Login

- Dashboard

- Lista de chamados

- Detalhes do chamado

- Base de conhecimento

- Administração

### **Diretrizes**

- Interface responsiva.

- Navegação intuitiva.

- Acessibilidade.


## **12. CRONOGRAMA MACRO**

| **Fase** | **Duração Estimada** |
| :-: | :-: |
| Levantamento de requisitos |  |
| Análise e planejamento |  |
| Prototipação |  |
| Desenvolvimento |  |
| Testes |  |
| Homologação |  |
| Implantação |  |
| Treinamento |  |


## **13. RISCOS DO PROJETO**

| **Risco** | **Impacto** | **Probabilidade** | **Mitigação** |
| :-: | :-: | :-: | :-: |
| Mudanças frequentes de escopo | Alto | Alta | Formalizar mudanças |
| Falta de engajamento dos usuários | Médio | Média | Realizar workshops |
| Atrasos nas integrações | Alto | Média | Planejamento antecipado |
| Baixa qualidade dos requisitos | Alto | Média | Validação contínua |


## **14. PREMISSAS E RESTRIÇÕES**

### **Premissas**

- Disponibilidade dos stakeholders.

- Infraestrutura adequada.

- Equipe dedicada.

### **Restrições**

- Orçamento disponível.

- Prazo de entrega.

- Recursos humanos.


## **15. ESTIMATIVA DE CUSTOS**

| **Item** | **Valor Estimado** |
| :-: | :-: |
| Desenvolvimento |  |
| Infraestrutura |  |
| Licenças |  |
| Integrações |  |
| Treinamentos |  |
| Manutenção |  |


## **16. CRITÉRIOS DE ACEITAÇÃO**

- Todos os requisitos obrigatórios implementados.

- Integrações homologadas.

- Testes aprovados.

- SLA validado.

- Aprovação formal dos stakeholders.


## **17. APROVAÇÕES**

| **Nome** | **Cargo** | **Data** | **Assinatura** |
| :-: | :-: | :-: | :-: |
|  |  |  |  |
|  |  |  |  |

