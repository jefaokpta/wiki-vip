# Iasmin — Painel de Marcos

A **Iasmin** (nome interno: VIP 2) é a nova plataforma da VIP Solutions. Ela reúne numa só solução, em nuvem, a central telefônica, o atendimento em filas, o telefone no navegador e relatórios. As próximas etapas trazem WhatsApp, videoconferência e inteligência artificial. A preparação para venda está prevista para **2027**.

Esta página mostra, para cada marco do produto, **o que se espera**, **o que já temos**, **o que está previsto** e **o que sugerimos melhorar**, tanto na plataforma quanto na equipe.

---

## Resumo

| Marco | Principal destaque hoje | Próximo passo sugerido |
|-------|-------------------------|------------------------|
| Visão do produto | Plataforma única com identidade Iasmin já aplicada | Documento de 1 página "o que é / o que não é", aprovado pela diretoria |
| Hipóteses validadas | Funcionalidades inspiradas em clientes reais do VIP | Listar de 3 a 5 hipóteses com critério de sucesso mensurável |
| Arquitetura | Base moderna, em nuvem, multiempresa e pronta para crescer | Teste de carga para saber quantas chamadas simultâneas suportamos |
| MVP | Núcleo de telefonia e atendimento já funcionando | Fechar o escopo e escolher as empresas piloto |
| Funcionalidades | Mais de 30 recursos prontos | Priorizar os próximos por impacto, esforço e dependência |
| Integrações | E-mail, armazenamento em nuvem e operadoras | Trazer para a Iasmin as integrações com CRM que o VIP já tem |
| Segurança | Várias camadas de proteção já ativas | Requisitos formais, incluindo LGPD para gravações |
| Testes | Testes automáticos e revisão obrigatória antes de cada entrega | Testes de ponta a ponta com ligações reais |
| Validação | Discovery com clientes iniciado pela equipe de produto | Piloto com clientes e registro das evidências |
| Documentação | Fluxos, decisões e padrões documentados | Base de conhecimento para suporte e usuários |
| Operação | Monitoramento, publicação automática e ferramentas de implantação | Fluxo de suporte e escalonamento com prazos definidos |
| Estabilidade | Métricas de saúde da plataforma já coletadas | Definir as metas de confiabilidade e acompanhá-las em painel |
| Go-To-Market | Cobrança por chamada, modelo de revenda e relatórios prontos | Planos, preços, material de vendas e checklist de lançamento |

---

## 1. Visão do produto

**O que se espera:** visão do produto documentada e aprovada pela direção, com um escopo claro do que é e do que não é a Iasmin.

**O que já temos**
- Conceito definido: uma plataforma única, em nuvem, que concentra as frentes da empresa (telefonia, atendimento, WhatsApp, conferências e IA).
- Identidade visual **Iasmin** já aplicada na interface.
- Roadmap público nesta wiki, com o que está pronto, em andamento e planejado.

**O que está previsto**
- Expansão para WhatsApp, videoconferência e inteligência artificial, conforme o [Roadmap](/docs/vip2/roadmap.md).

**Sugestões de melhoria**
- Escrever um documento curto, de uma página, com: para quem é a Iasmin (perfil de cliente ideal), qual problema resolve, principais diferenciais e **o que ela não é** no primeiro momento (por exemplo, não é um CRM completo).
- Levar esse documento para aprovação formal da diretoria e revisá-lo a cada trimestre.

---

## 2. Hipóteses validadas

**O que se espera:** hipóteses prioritárias definidas, cada uma com critérios de validação.

**O que já temos**
- As funcionalidades atuais partiram da experiência com os clientes do VIP, ou seja, de necessidades reais já conhecidas.
- A plataforma já oferece recursos que servem de base para testar hipóteses: telefone no navegador, relatórios de atendimento, pesquisa de satisfação e gestão por revendas.

**O que está previsto**
- Até aqui as decisões vieram mais da visão interna e de mercado. A partir de agora, as prioridades passam a ser definidas ouvindo clientes.

**Sugestões de melhoria**
- Registrar de 3 a 5 hipóteses, cada uma com um critério objetivo. Exemplos:
  - *"Empresas pagam mais por um telefone que funciona dentro do CRM"*: validada se X de cada 10 clientes entrevistados demonstrarem interesse de compra.
  - *"Transcrição e resumo automático das ligações por IA é motivo de troca de fornecedor"*.
  - *"Revendas querem administrar as próprias empresas clientes"*.
- Anotar para cada hipótese: dono, prazo e resultado (confirmada, ajustada ou descartada).

---

## 3. Arquitetura

**O que se espera:** arquitetura definida, documentada e validada tecnicamente quanto à capacidade de suportar o MVP e a evolução futura.

**O que já temos**
- Plataforma em nuvem, dividida em partes independentes que conversam entre si. Se uma parte precisa crescer, ela cresce sem afetar as outras.
- Capacidade de aumentar o volume de chamadas adicionando novos servidores de processamento.
- **Multiempresa com hierarquia**: plataforma → revenda/parceiro → empresa cliente, cada uma com seus dados isolados.
- Mudanças de configuração valem na hora, sem reiniciar nada e sem derrubar ligações.
- Instalação padronizada em containers, o que facilita subir novos ambientes.
- As decisões técnicas importantes ficam registradas por escrito antes de serem implementadas.

**O que está previsto**
- Cadastro dos servidores de processamento de chamadas pela própria interface.

**Sugestões de melhoria**
- Fazer um **teste de carga** para descobrir quantas ligações simultâneas cada servidor suporta. Esse número é importante para a precificação e para o comercial.
- Manter um desenho simples da arquitetura, de uma página, para apresentar a parceiros e clientes corporativos.

---

## 4. MVP

**O que se espera:** MVP definido em escopo, critérios de sucesso, funcionalidades essenciais, dependências, roadmap de desenvolvimento e escolha das empresas para o início.

**O que já temos**
- O núcleo de um MVP de telefonia com atendimento já funciona: ramais, filas, menu de voz (URA), telefone no navegador, painéis ao vivo, relatórios de chamadas e de atendimento, pesquisa de satisfação e cobrança por chamada.
- Ambiente de homologação ativo e ambiente de produção em preparação.

**O que está previsto**
- Concluir a gravação de chamadas, que é a base para os recursos futuros de IA.

**Sugestões de melhoria**
- Fechar formalmente o recorte do MVP. Sugestão: **telefonia + filas + telefone no navegador + relatórios + pesquisa de satisfação**, com IA na fase seguinte.
- Escolher de **2 a 3 empresas piloto** entre os clientes atuais do VIP, de perfis diferentes (por exemplo: um call center pequeno e um escritório com vários ramais).
- Definir critérios de sucesso do piloto: ligações sem falha, satisfação dos usuários e tempo de implantação.

---

## 5. Funcionalidades

**O que se espera:** o conjunto mínimo de funcionalidades do MVP definido e priorizado por impacto, dependência e esforço.

**O que já temos**
- **Telefonia:** ramais (inclusive criação em lote), grupos de chamada, captura, transferência, estacionamento de chamada, rotas, números de entrada, horários de funcionamento e feriados, áudios e músicas de espera personalizados.
- **Atendimento:** filas com diferentes formas de distribuir as chamadas, entrada e saída de atendentes, pausas, tempo de descanso entre atendimentos, e supervisor que desloga atendentes.
- **Acompanhamento:** painéis ao vivo de ramais e filas.
- **Relatórios:** chamadas com gráficos e custo, detalhe de cada chamada, jornada completa da ligação com áudio sob demanda, atividade dos atendentes (tempo em pausa, atendimentos do dia) e resultados das pesquisas de satisfação.
- **Pesquisa de satisfação** automática ao final do atendimento.
- **Telefone no navegador (WebPhone)**, sem necessidade de aparelho físico.
- **Gestão:** empresas, revendas, usuários e perfis de acesso.
- **Numeros:** Mais de 30 funcionalidades até o momento.

**O que está previsto**
- Conclusão da gravação de chamadas.
- Módulo Inteligência artificial: transcrição de ligações, análise de sentimento e agentes virtuais.
- WhatsApp (mensagens, histórico).
- Módulo de vendas
- Módulo Videoconferência.
- CRM VIP e hub de integrações.


**Sugestões de melhoria**
- Montar uma matriz simples **impacto × esforço × dependência** para os itens previstos.
- Respeitar a ordem natural das dependências: **gravação → transcrição → inteligência** (resumo, sentimento, indicadores).

---

## 6. Integrações

**O que se espera:** mapa das integrações críticas definido, com prioridade, dependência, esforço e estratégia técnica para o MVP.

**O que já temos**
- Envio automático de e-mails (confirmação de cadastro e recuperação de senha).
- Armazenamento dos áudios em nuvem S3.
- Conexão com operadoras de telefonia para ligações externas.
- Experiência acumulada no VIP atual com integrações de CRM (Agendor, Kommo, Pipedrive, PipeRun, ExactSales) e ligação por clique ("click to call"). Veja a seção *Integrações* desta wiki.

**O que está previsto**
- Hub de integrações VIP.
- WhatsApp.
- Videoconferência (sem servidor ou notetaker).


**Sugestões de melhoria**
- Trazer para a Iasmin, em primeiro lugar, as integrações com CRM que os clientes do VIP já usam. Um **telefone integrado ao CRM** é um diferencial visível para vendas.
- Oferecer uma API pública documentada, para que parceiros criem as próprias integrações.
- Registrar o mapa de integrações numa tabela única: integração, prioridade, esforço e de quem depende.

---

## 7. Segurança

**O que se espera:** requisitos de segurança definidos e incorporados à arquitetura.

**O que já temos**
- Login com senha protegida e confirmação de cadastro por e-mail.
- Limite de tentativas no login, na confirmação e na recuperação de senha, contra ataques de adivinhação.
- Perfis de acesso por nível (plataforma, revenda, empresa, atendente).
- Dados de cada empresa totalmente isolados dos dados das outras.
- **Bloqueio automático** de endereços que tentam invadir a telefonia.
- Senhas dos ramais guardadas de forma criptografada.
- Comunicação entre as partes internas da plataforma protegida por autenticação.
- Telefone no navegador com conexão criptografada.

**O que está previsto**
- Registro (auditoria) das ações do suporte quando ele acessa a conta de um cliente.

**Sugestões de melhoria**
- Reunir tudo isso num **documento formal de requisitos de segurança**.
- Tratar a **LGPD** desde já. Gravações e transcrições são dados pessoais e precisam de regras de retenção, consentimento e exclusão.
- Contratar um teste de invasão externo antes do lançamento comercial.
- Oferecer verificação em duas etapas no login para administradores.

---

## 8. Testes

**O que se espera:** estratégia de testes definida e incorporada ao processo de desenvolvimento do MVP.

**O que já temos**
- Testes automáticos em todas as partes da plataforma, com mais de 120 conjuntos de testes (~500).
- Regra da equipe: uma entrega só é considerada pronta **com todos os testes passando**.
- **Revisão obrigatória** de cada alteração por um revisor independente (agente de IA) antes de ser integrada.
- Ambiente de homologação separado, com publicação automática, para validar antes da produção.

**O que está previsto**
- Mais testes automáticos na interface (telas).

**Sugestões de melhoria**
- Criar testes de ponta a ponta que **simulem ligações reais** (atender, transferir, entrar na fila).
- Fazer testes de carga periódicos.
- Montar um roteiro de testes de aceitação para as empresas piloto.

---

## 9. Validação

**O que se espera:** hipóteses críticas submetidas a usuários e potenciais clientes, gerando evidências para confirmar, ajustar ou descartar decisões do produto.

**O que já temos**
- A equipe de produto começou a fazer **discovery**, ou seja, conversas estruturadas com clientes.
- As funcionalidades mais usadas no VIP atual já foram validadas por seus usuários e serviram de base para a Iasmin.

**O que está previsto**
- Validação com empresas piloto após a definição do MVP.

**Sugestões de melhoria**
- **Equipe:** tornar o discovery uma rotina, com pelo menos **uma pesquisa com cliente documentada por mês**, conduzida pela equipe de produto sem depender da área técnica.
- Envolver o **Comercial e o Suporte** nas conversas: eles ouvem os clientes todos os dias.
- Registrar tudo no formato **hipótese → evidência → decisão**, para a diretoria acompanhar.
- Fazer demonstrações da Iasmin para clientes selecionados e coletar a reação de forma estruturada.

---

## 10. Documentação

**O que se espera:** garantir que o conhecimento crítico da Iasmin não fique na cabeça de uma única pessoa.

**O que já temos**
- Esta wiki, com a visão geral, o roadmap e esta página.
- Os **fluxos de ligação** (chamada simples, transferências, filas e grupos) documentados passo a passo.
- Toda funcionalidade nova tem **especificação escrita antes de ser construída** e as decisões técnicas ficam registradas.
- Os padrões de trabalho da equipe estão escritos, inclusive para os assistentes de IA usados no desenvolvimento.
- Registro de problemas conhecidos e de como contorná-los.

**O que está previsto**
- Manter a wiki atualizada a cada marco concluído.

**Sugestões de melhoria**
- **Equipe:** cada pessoa passa a ser responsável por documentar a sua área (produto, infraestrutura, desenvolvimento). Documentar deve fazer parte da definição de "pronto".
- **Equipe:** fazer rodízio. Quem não construiu uma parte deve conseguir mantê-la usando apenas a documentação, e isso serve de teste.
- Documentar as **regras de negócio** (cobrança, filas, horários) em linguagem simples.
- Criar um **manual do usuário** e uma **base de conhecimento para o suporte**.

---

## 11. Operação

**O que se espera:** modelo operacional mínimo definido para venda, implantação, suporte, manutenção e escalonamento.

**O que já temos**
- **Monitoramento ativo:** painéis de saúde da plataforma e registro centralizado de eventos, para agir antes que o cliente perceba um problema.
- **Publicação automática** das atualizações no ambiente de homologação.
- **Implantação ágil:** criação de empresas e de ramais em lote pela própria interface.
- **Suporte:** a equipe da plataforma pode entrar na conta do cliente para ajudar.
- **Modelo de revenda:** parceiros podem administrar as próprias empresas clientes.

**O que está previsto**
- Ambiente de produção em nuvem em fase final de preparação.

**Sugestões de melhoria**
- **Equipe de suporte:** treinar o primeiro nível na Iasmin antes do piloto.
- Definir o **fluxo de escalonamento** (suporte → especialista → desenvolvimento), com RCA e prazos de resposta.
- **Equipe de infraestrutura:** evoluir de "reagir a incidentes" para "avisar antes", com alertas automáticos, e propor otimizações de custo de nuvem por iniciativa própria.
- Criar um **roteiro de implantação** padrão, com checklist, para novos clientes.
- **Comercial:** preparar demonstração, material de apoio e treinamento sobre a Iasmin.

---

## 12. Estabilidade

**O que se espera:** requisitos de estabilidade definidos, com indicadores que permitam medir a confiabilidade do MVP.

**O que já temos**
- Métricas de saúde de todas as partes da plataforma coletadas continuamente.
- Registro centralizado de eventos para investigar falhas.
- Atualizações de configuração aplicadas sem derrubar ligações.
- Correções recentes de robustez em filas, transferências e menu de voz.

**O que está previsto**
- Painéis de monitoramento em uso pela equipe de infraestrutura (Grafana).

**Sugestões de melhoria**
- Definir as metas de confiabilidade. Exemplos: **disponibilidade de 99,9%**, **taxa de ligações completadas**, **qualidade de áudio** e **tempo para recuperar de uma falha**.
- Exibir esses indicadores num painel acompanhado semanalmente.
- Criar uma **página de status** para clientes e para o suporte.

---

## 13. Go-To-Market

**O que se espera:** tudo pronto para vender com segurança.

**O que já temos**
- Marca e identidade visual **Iasmin** aplicadas.
- **Cobrança por chamada** calculada automaticamente, com custo por ligação nos relatórios.
- Estrutura **multiempresa e de revendas**, que permite vender direto ou por parceiros.
- Diferenciais visíveis para demonstração: telefone no navegador sem aparelho, painéis ao vivo, relatórios completos de atendimento e pesquisa de satisfação automática.

**O que está previsto**
- Recursos de IA (transcrição e análise das ligações), vistos como principal diferencial futuro.
- Integração do telefone com CRMs via hub de integrações.
- Transcrições de videochamadas via P2P ou notetaker (implementações proprias) 

**Sugestões de melhoria**
- Definir **planos e preços**, por exemplo por ramal, por atendente ou por pacote com IA.
- Criar o **material de marketing**: página da Iasmin, apresentação comercial e vídeo de demonstração.
- Preparar contratos e termos de uso, incluindo LGPD.
- Montar um **checklist de lançamento** que só é aprovado quando piloto, suporte, estabilidade e material de vendas estiverem prontos.
- Colher **casos de sucesso** das empresas piloto para usar na divulgação.

---

*Última atualização: setembro de 2026*
