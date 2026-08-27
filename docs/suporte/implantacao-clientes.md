# Implantação de clientes - EM CONSTRUÇÃO

## Objetivo

Passo-a-passo de como configurar de um novo cliente do zero no VIP.

## Ticket de Implantação

Todas as informações do novo cliente ficam registradas via ticket no sistemas de chamado, ele deve ter:

- **Informações do servidor**
    - Servidor escolhido
    - Código da empresa e do servidor (Control number)

- **Informações de contato do cliente**
    - Nome
    - E-mail
    - Telefone

- **Informações de configuração dos ramais**
    - Range de ramais padrão ou personalizado (formulário em anexo)
    - Se será configurado em Softphone (PC ou Celular) ou Telefone IP
    - Quantidade de ramais
    - Quantidade de ramais móveis

- **Informações de configuração do WIP**
    - Se o cliente vai utilizar ou não o serviço
    - Versão Beta ou Oficial (Meta)
    - Se possui um aparelho para ativação do WhatsApp Business
    - Tipo de número a ser utilizado (Fixo ou Celular)
    - Número que será utilizado

- **Informações de Integração**
    - Se fará uso de integração
    - Se sim, qual integração será utilizada (homologada e já nativa no VIP ou alguma API externa específica para o cliente)

- **Informações de configuração de números de Entrada (DDR)**
    - Números de Entrada (DDD+NUM)
    - Quantidade de canais simultâneos contratados
    - Origem (portado de outra operadora ou VIP)
    - Data e hora da portabilidade (para números portados)
    - Operadora (VIP ou outras operadoras)

- **Informações de configuração de Atendimento e URA**
    - Se vai ou não usar URA
    - Tipo de URA (Apenas saudação ou completa com opções de direcionamento)
    - Se vai utilizar áudio para ligações de fora do horário
    - Se tem turno 24hrs
    - Se fará uso de filas de atendimento

- **Informações de DNS**
    - Padrão de DNS a ser criado para o cliente (admin, login e sip)

- **Observações**
    - Anexar gravações de treinamento
    - Eventuais particularidades do cliente

- **Anexos**<br>
 O anexo possui informações preenchidas pelo cliente


## Iniciando as configurações

Acesse o painel VIP correspondente ao servidor definido pela equipe de implantação, depois no menu lateral esquerdo > Gerenciar > Gerenciar uma Empresa, selecione a empresa do novo cliente.

### Cadastrar Ramais

Tela `Cadastros > Ramais`, nessa tela é necessário configurar:

- Ramais individualmente ou em lote
- O dispositivo que será utilizado: Telefone IP, Celular, PC, MicroVIP (VipPhone)
- Senha de registro (importante definir uma senha dificil com letras e números)
- Nome do ramal (para configuração individual)
- Senha do ramal (senha de funcionalidade)
- Qualidade (monitoria do ramal no VIP)
- Ordem dos codecs (Ulaw, Alaw, g729)
- Utiliza NAT em **sim**
- Bina (número que aparece nas chamadas realizadas pelo ramal)

### Grupo de chamada

Tela `Cadastros > Grupo de chamada`

Caso o cliente necessite que uma chamada toque em um conjunto de ramais específicos, crie um grupo de chamada definindo:

- Nome de grupo (facilitando a identificação na configuração das regras de discagem)
- Música de espera (áudio que o cliente ouve enquanto aguarda atendimento)
- Anúncio (Também conhecido como sussurro, é um áudio reproduzido ao atender a chamada, identificando a origem da mesma)
- Ramais (selecione os ramais que podem receber chamadas desse grupo)
- Grupo de captura (permite a captura de chamadas para os ramais vinculados ao grupo)
- Tipos de toque
    - Tocar em todos (toca em todos os ramais selecionados)
    - Tocar aleatóriamente (toca aleatóriamente em qualquer um dos ramais selecionados)
    - Tocar no mais ocioso (toca no ramal que está a mais tempo sem atender chamadas)
- Tempo de toque
    - É o tempo que a chamada toca em um ramal do grupo antes de começar a tocar em outro (nos casos do aleatoriamente ou mais ocioso)

### Grupo de captura

Tela `Cadastros > Grupo de captura`

Crie o(s) grupo(s) e depois vincule(os) aos ramais a partir da coluna `Membros` 

O grupo de captura serve para que um ramal possa capturar uma chamada de outro ramal que faz parte do mesmmo grupo com o comando `*8`, esse comando puxa a chamada de outro ramal para o ramal que executou o comando

#### Membros

Clique na lupa (&#128269;) do grupo para selecionar quais ramais serão vinculados ao mesmo.

### Músicas do Sistema

Tela `Cadastros > Músicas do Sistema`

Para clientes que desejam configurar um áudio de espera para fila de atendimento, grupo de chamada, URA de atendimento, o arquivo de áudio deve ter os seguintes parâmetros:

- Canal Mono
- Frequência de 8000Hz
- 16-bit
- Wave (.wav)

Observação: o nome do arquivo não deve possuir caracteres especiais, no lugar dos espaços, utilizar o _ (underline) entre as palavras, assim o asterisk não terá problema em fazer a conversão do áudio para o asterisk ler e reproduzir na ligação.

### Rotas

Tela `Cadastros > Rotas`

Crie uma rota para ligações saintes, geralmente nomeada como `SAIDAS_JUPITER` ou qualquer outra nomenclatura que remeta a uma operadora contratada pelo cliente ou algum servidor VIP específica.

### Rotas (Ordem)

Tela `Cadastros > Rotas (Ordem)`

Selecione a rota criada anteriormente e defina o tronco de saída, sempre na coluna `TRONCO 1`, os centro de custos que devem ter um tronco vinculado são:

- LOCAL
- DDD
- VC1
- VC2
- FUNCIONALIDADE

Observação: não é todo cliente que contrata o completamento `DDI`, mas se for o caso, uma rota `SAIDAS_DDI` deve ser criada em pararelo e o tronco deve ser vinculado ao centro de custos `DDI`

### Usuários

Tela `Cadastros > Usuários`

Crie os acessos baseado nas informações preenchidas no formulário anexado no ticket, informe:

- Nome
- Email (utilizado na recuperação de senha)
- Login (individual e único)
- Senha
- Perfil (o VIP tem a descrição do que cada perfil tem acesso)

Observação: os usuários listados no formulário já devem ter o perfil selecionado.

### Uras do Sistema

Tela `Cadastros > Uras do Sistema`

Para clientes que desejam uma URA de atendmimento, a mesma deve ser criada com:

- Título (Para fácil identificação no relatório `Estatítiscas da URA` e na configuração de regras de discagem)
- Tempo de duração em 2
- Tempo de digitação em 2
- Mensagem da URA (áudio cadastrado anteriormente na tela `Músicas do Sistema`)

#### Customização da URA

- Redirecionamento na discagem de número inválido ou tempo excedido
- Discar ramal desejado
- Opções de atendimento

#### Ações da URA

- Desligar: desliga a chamada ao discar alguma opção
- Voltar ao inicio: reproduz o áudio da URA novamente
- Discar para ramal: direciona a chamada para o ramal selecionado
- Grupo de chamada: direciona para um grupo de chamada/ramais
- Desvio Especial: direciona a chamada para uma regra de discagem com a mesma expressão regular da ação
- Sub-URA: direciona a chamada para um sub-menu com outras opções de discagem
- WhatsApp: Permite iniciar uma conversa no sistema WIP (WhatsApp da VIP)

### Agentes

Para clientes que desejam utilizar filas de atendimento, os agentes devem ser criados antes das filas, com isso os agentes poderão efetuar o login em fila para receber ou efetuar chamadas e atender contatos que interagirem com chatbots do WIP.

Tela `Cadastros > Agentes`, nessa tela é necessário configurar:

- Agentes individualmente ou em lote
- Agente: código de 4 digitos, geralmente o range padrão é 9XXX, mas alguns servidores tem o range par (2XXX, 4XXX, 6XXX e 8XXX)
- Nome: Cadastrar Primeiro nome e no máximo 1 sobrenome para fácil identificação na tela `Gerenciar Filas` e nos relatórios `DAC Sintético/Analítico, Estatísticas/Atividades do Agente`
- Email: Opcional
- Senha: utilizada para logar no sistema de fila

### Filas

Para clientes que desejam utilizar filas de atendimento, possibilitando receber chamadas

Tela `Cadastros > Filas`

- Título (Para fácil identificação na tela `Gerenciar Filas` e nos relatórios `DAC Analítico/Sintético`)
- Música de espera (áudio que o cliente ouve enquanto aguarda atendimento)
- Grupo de captura (permite a captura de chamadas para os ramais vinculados ao grupo)
- Anúncio (Também conhecido como sussurro, é um áudio reproduzido ao atender a chamada, identificando a origem da mesma)
- Entrar em fila
- Tempo de toque
    - É o tempo que a chamada toca em um ramal do grupo antes de começar a tocar em outro (nos casos do aleatoriamente ou mais ocioso)

#### Comportamento da Fila
- **Entrar em fila vazia (padrão inicial: não)**
    - Não: impede que chamadas entrem na fila quando a mesma estiver sem nenhum agente logado
    - Sim: permite que chamadas entrem na fila mesmo sem nenhum agente logado
    - Não (se todos os agentes estiverem em pausa): impede que chamadas entrem na fila quando todos os agentes estiverem em pausa
<br><br>
- **Sair de fila vazia (padrão inicial: sim)**
    - Não: impede que as chamadas saiam da fila caso os agentes desloguem da mesma
    - Sim: chamadas que estiverem em espera na fila sairão da mesma caso os agentes desloguem da mesma
    - Sim (se todos os agentes estiverem em pausa): chamadas que estiverem em espera na fila sairão quando todos os agentes entrarem em pausa
<br><br>
- **Distruibuir Chamadas**
    - Tocar em todos (ringall): toca em todos os agentes disponíveis (a menos que estejam com DND ativo no ramal)
    - Tocar no mais ocioso (leastrecent): toca no agente que está a mais tempo sem atender chamadas
    - Aletoriamente (random): toca aleatoriamente em qualquer agente que esteja disponível
    - Igualmente (rrmemory): Distribui as chamadas entre os agentes e lembra o ultimo agente que tentou chamar caso o mesmo não tenha atendido
    - Tocar no que menos atendeu (fewestcalls): Toca no agente que atendeu menos chamadas
<br><br>
- Gravar fila: sempre em **sim**

#### WhatsApp
- WhatsApp - Departamentos: Permite a seleção de um departamento do WIP para receber os contatos que interagirem com chatbots do WIP.

#### Membros

Clique na lupa (&#128269;) do fila para selecionar quais agentes serão vinculados a mesma.

Observação: um agente pode fazer parte de mais uma fila, seja para receber chamadas ou fazer uso do WIP.

### Status do Agente

Tela `Cadastros > Status do Agente`

Essa tela permite cadastrar diferentes status de pausa para uso dos agentes no sistema de filas.

Esse status fica visivel para o agente na coluna `modificar status` quando está logado em uma ou mais filas, ao selecionar a pausa, a mesma fica visivel para o gestor na tela `Gerenciar > Gerenciar Filas` e também é registrada no relatório `Atividades do Agente`

### Pesquisa de Satisfação

Tela `Cadastros > Pesquisa de Satisfação`

Para clientes que desejam reproduzir uma pesquisa de satisfação no final de uma ligação atendida por um ramal/agente, nessa tela é necessário configurar:

- Título: para fácil identificação no relatório `Pesquisa de Satisfação` e nas `regras de discagem de Entrada`
- Áudios: cadastrados em `Músicas do Sistema` 
    - Saudação: áudio de saudação que o cliente ouve ao entrar na pesquisa
    - Pergunta 1,2 e 3: pode registrar digitos de 1 até 5, com o intúito de registrar notas de atendimento no geral
    - Agradecimento: áudio que o cliente ouve após registrar uma nota nas perguntas

## Alias de Discagem

Tela `Regras > Alias de Discagem`

Nessa tela cadastramos o plano de discagem da empresa, permitindo o completamento de ligações saintes com diferentes formatos de discagem, segue abaixo um exemplo de Alias:

- FIXO_LOCAL_11
    - |[2-5]XXXXXXX
    - 11|[2-5]XXXXXXX
    - 011|[2-5]XXXXXXX
    - 5511|[2-5]XXXXXXX
<br><br>
- CELULAR_LOCAL_11
    - |9XXXXXXXX
    - 11|9XXXXXXXX
    - 011|9XXXXXXXX
    - 5511|9XXXXXXXX
<br><br>
- FIXO_DDD
    - |ZZ[2-5]XXXXXXX
    - |ZZ[2-5]XXXXXXX
    - 55|ZZ[2-5]XXXXXXX
<br><br>
- CELULAR_DDD
    - |ZZ9XXXXXXXX
    - 0|ZZ9XXXXXXXX
    - 55|ZZ9XXXXXXXX
<br><br>
- GRATUITAS
    - 102XX
    - 103XX
    - 106XX
    - 1XX
    - 0800.

Esse padrão pode ser criado de forma automática usando o botão `Alias Padrão` na mesma tela, nela é necessário informar somente o `DDD de origem do cliente`, informando o `DDD 21` por exemplo, todo o padrão de alias acima será criado com o `DDD 21`.