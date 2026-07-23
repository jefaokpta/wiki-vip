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

Inicie as configurações criando os ramais