# Sistema VIP

---

### DESCRIÇÃO

O sistema Vip foi lançado no dia 01 de Dezembro de 2014, tendo como objetivo proporcionar o gerenciamento de toda infraestrutura telefônica (VoIP) de uma empresa.

A necessidade de tornar tarefas complexas em atividades simples, que possam ser executadas de qualquer lugar do mundo — bastando estar conectado à Internet — faz o sistema Vip possibilitar que os gestores tenham em tempo real o panorama completo de seu sistema telefônico: monitorar chamadas, participar de conferências telefônicas, gravar ligações e posteriormente ouvir esses áudios para analisar os atendimentos prestados, criar URA's de pré-atendimento, filas para distribuição de chamadas personalizadas, pesquisa de satisfação dos clientes, além de diversos relatórios que poderão auxiliar em possíveis tomadas de decisões importantes.

Entendendo que cada cliente/empresa possui necessidades distintas, o sistema Vip também pode ser customizado, em versões exclusivas e com toda a infraestrutura completamente separada e dedicada. É possível também realizarmos integrações com diversos CRM's, melhorando desta forma o acesso à telefonia e posteriormente, enviando os dados da chamada para que fiquem anexados ao histórico dos contatos/Leads.

Existe também a possibilidade de customização do layout do sistema para os clientes licenciados, ou seja, é possível que o sistema Vip tenha as cores e logotipos de outras empresas, visando assim estabelecer parcerias e fazendo o sistema chegar a diversos lugares em todo o país.

---

### SISTEMA DE GESTÃO DE TELEFONIA (VIP)

O sistema Vip é estruturado em dois módulos específicos, sendo um responsável por gerir toda a parte técnica (configurações da telefonia), sendo este totalmente restrito aos profissionais habilitados para manipulá-lo, e o outro é voltado para gestão e acompanhamento da utilização da telefonia pelos usuários/empresas.

Em ambos os módulos, existem 4 níveis hierárquicos independentes, possibilitando que recursos específicos sejam disponibilizados a estes perfis de usuários de formas distintas.

---

### MÓDULOS TÉCNICOS

#### CADASTROS

**Empresas**
O sistema Vip é um sistema Multi-Tenancy, palavra de origem inglesa que significa "múltiplos inquilinos", ou seja, ele possui uma arquitetura que atende a vários clientes/empresas ao mesmo tempo. Logo, o cadastro de uma empresa é a primeira etapa a ser realizada. A partir da conclusão deste cadastro, todas as configurações efetuadas para esta empresa serão completamente restritas e aplicadas somente a ela.

**Troncos**
Os troncos (SIP / IAX2 / E1 / Manual) quando contratados e devidamente cadastrados, permitem que ramais efetuem e/ou recebam chamadas telefônicas. Um tronco é um importante elemento quando deseja-se criar regras para saída de ligações.

**Troncos (Valores)**
Os valores das tarifas dos troncos poderão ser cadastrados para consultas posteriores. Esta opção é meramente informativa, não tendo nenhum impacto caso não seja utilizada, porém, será imprescindível se desejar posteriormente extrair o relatório "Detraf".

> **Obs:** Os troncos cadastrados são a única coisa no sistema Vip que poderão ser utilizados em qualquer empresa existente no servidor, e isso por um motivo bem lógico: ele não enxergará detalhes da nossa aplicação, apenas será um canal por onde nosso servidor poderá realizar as chamadas VoIP.

**Rotas**
As rotas são utilizadas para indicar o trajeto de destino de uma chamada, ou seja, o caminho (tronco) onde uma chamada será efetuada.

**Rotas (Ordem)**
É comum termos mais de uma rota de saída para realizarmos chamadas. Nesta opção, é possível indicarmos qual rota terá prioridade nas tentativas, assim como é possível definir outras opções caso a alternativa principal apresente alguma indisponibilidade.

**Operadoras**
Essa é mais uma opção informativa, onde você poderá cadastrar os dados de uma determinada operadora contratada. A utilização deste recurso não é obrigatória e também não impactará em nada a utilização do sistema.

**Ramais**
Um ramal é criado para permitir que um usuário possa receber e efetuar ligações. Ele poderá ser utilizado em um aparelho SIP ou mesmo em um softphone, que pode ser instalado tanto em um PC quanto em um celular (Smartphone).

---

#### REGRAS

**Centro de Custo**
Tem por objetivo identificar os tipos de chamadas a serem efetuadas ou recebidas. Ex: Telefone Fixo - Local, Telefone Fixo - DDD, Telefone Móvel - Local, Telefone Móvel DDD, Chamada Internacional, Chamadas de Entrada (sem cobrança), Chamadas de Entrada 0800 / 400X (com cobrança), além das chamadas internas e gratuitas (de um ramal para outro ramal).

**Alias de Discagem**
Serve para indicar um conjunto de possibilidades que deverão ser tratadas como um único identificador em uma regra de discagem, evitando assim a necessidade de regras complexas e de difíceis interpretações.

**Regras de Discagem**
Uma das mais importantes configurações do sistema, afinal, são estas regras que orquestrarão todo o caminho de uma ligação, seja de entrada ou saída. Aqui definimos tudo o que deverá ser considerado ao receber ou efetuar uma determinada chamada.

**Bloqueio de Discagem**
Este recurso possibilita que um ou vários ramais sejam impedidos de efetuarem chamadas para um determinado tipo de centro de custo.

---

#### CONFIGURAÇÕES

**DDR**
Nesta opção poderemos cadastrar os números de recebimento de chamadas VoIP.

**Perfil de Acesso Externo**
Permite criar um perfil que poderá conter um valor pré-definido para permitir que um determinado usuário (IP) efetue chamadas pelo servidor, mesmo estando a uma considerável distância física da empresa.

**Acesso Externo**
Permite informar um IP e vinculá-lo a um perfil de acesso externo, determinando de forma específica de onde um ramal conseguirá se registrar para efetuar chamadas pelo sistema.

**Bloquear Acesso**
Permite cadastrar um IP que não poderá se comunicar com o servidor VoIP.

**Permitir Acesso**
Permite que um determinado IP consiga se comunicar com o servidor VoIP.

---

#### SUPORTE

**Ramais Ativos**
Possibilita inspecionar um ramal, verificando em tempo real o IP registrado, tempo de registro no IP atual, se o ramal está com funções como "não perturbe" ou "siga-me" ativadas, entre outras.

**Log Asterisk**
Permite analisar os logs gerados pelo servidor Asterisk (VoIP) em tempo real.

**Simular Chamada**
Possibilidade de indicar uma origem e um número de destino, e verificar qual será a regra escolhida naquele momento para realizar tal chamada.

**Log de Atividades**
Permite indicar um período e verificar algumas ações dos usuários realizadas, como por exemplo quem criou um determinado ramal ou quem excluiu uma regra de discagem do sistema.

---

#### RELATÓRIOS

**ASR**
O termo ASR é um acrônimo na língua inglesa das palavras "Answer Seizure Ratio", que podemos interpretar como uma análise dos resultados/atendimentos das chamadas efetuadas. Este relatório mostra a quantidade de chamadas efetuadas por um determinado tronco e, consequentemente, exibe a taxa/percentual de chamadas atendidas e rejeitadas.

**Detraf**
O termo Detraf é o acrônimo para "Declaração de Tráfego", ou seja, este relatório visa mostrar o quanto foi gasto com as operadoras/troncos. Ele poderá ser utilizado para confrontar os valores dos relatórios fornecidos pelas operadoras VoIP contratadas em um determinado período. Como mencionado anteriormente, para que este relatório possa comprovar a sua eficácia, será imprescindível o cadastramento dos valores negociados com as operadoras no formulário relacionado a cada tronco ("Troncos (Valores)").

> **Obs:** Estes relatórios contemplam os dados de todas as chamadas realizadas em um determinado período e abrangem todas as empresas cadastradas no servidor. Afinal, independente do número de empresas que tenhamos em um servidor Vip, a cobrança realizada pelas operadoras sempre será única, também correspondendo apenas a um ciclo mensal.
