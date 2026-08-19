# Guia de Integração - WhatsApp Business API para Sistema Wip

## Introdução

A VIP Solutions apresenta este guia para configuração segura da integração com a **WhatsApp Business API (WABA)** para utilização do sistema Wip.

---

## 1 - PRÉ-REQUISITOS

Para iniciar, você precisará acessar dois serviços da Meta:

- https://business.facebook.com
- https://developers.facebook.com

Certifique-se que os acessos estejam disponíveis fazendo login em ambos os endereços.

### Criando um Portfólio Empresarial

Acreditamos que sua empresa já possua uma conta empresarial com o Facebook. Caso ainda não tenha, acesse https://business.facebook.com para criar um "Portfólio empresarial".

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/1.jpg "API WhatsApp Business")



**Informações necessárias:**
- Nome do portfólio (será o nome público da sua empresa na Meta - sem caracteres especiais)
- Nome e sobrenome
- Email comercial

Após preencher as informações, clique em **Criar**. Nas próximas duas janelas que aparecerem, clique em **Avançar** e por último, clique em **Confirmar**.

---

## 2 - CONFIGURAÇÕES

### Criando a Aplicação

1. Acesse https://developers.facebook.com
2. Após fazer login, clique em **"Meus apps"**
3. Clique em **"Criar aplicativo"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/2.jpg "API WhatsApp Business")

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/3.jpg "API WhatsApp Business")

### Preenchendo Dados do Aplicativo

1. Informe o **nome da aplicação** e seu **email comercial**
2. Clique em **"Avançar"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/4.jpg "API WhatsApp Business")

3. Na tela seguinte, clique em **"Outros"** (nas opções à esquerda) e também em **"Outro"** (na última opção à direita)
4. Clique em **"Avançar"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/5.jpg "API WhatsApp Business")

### Selecionando Tipo de Aplicativo

1. Selecione **"Empresa"**
2. Clique em **"Avançar"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/6.jpg "API WhatsApp Business")

### Informações da Empresa

1. Informe o **nome do app**, o **email de contato** e selecione o **portfólio empresarial** (criado nos pré-requisitos)
2. Clique em **"Criar Aplicativo"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/7.jpg "API WhatsApp Business")

### Configurando o WhatsApp

1. Na próxima tela, clique em **"Configurar"** na opção do **"WhatsApp"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/8.jpg "API WhatsApp Business")

2. Na tela seguinte, clique em **"Continuar"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/9.jpg "API WhatsApp Business")

---

## 3 - CRIANDO USUÁRIO ADMINISTRADOR

O próximo passo é criar um usuário de perfil **"Administrador"** para gerenciar o aplicativo. Isso será feito na conta business em https://business.facebook.com.

### Passo a Passo

1. Selecione seu **"Portfólio empresarial"** indicado na criação do aplicativo
2. Clique na opção **"Usuários do sistema"** (à esquerda)
3. Clique em **"Adicionar"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/10.jpg "API WhatsApp Business")

4. Aceite os termos do Facebook para prosseguir
5. Informe o **nome do usuário** a ser criado
6. Escolha a opção **"Admin"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/11.jpg "API WhatsApp Business")

7. Clique em **"Atribuir ativos"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/12.jpg "API WhatsApp Business")

### Atribuindo Permissões

1. Na tela seguinte, clique em **"Selecionar tudo"**
2. Selecione **"Gerenciar app"** na opção **"Controle Total"**
3. Clique em **"Atribuir ativos"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/13.jpg "API WhatsApp Business")

4. Clique em **"Concluir"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/14.jpg "API WhatsApp Business")

---

## 4 - GERANDO TOKEN DE ACESSO

1. De volta à tela inicial, clique em **"Gerar token"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/15.jpg "API WhatsApp Business")

2. Na tela seguinte, selecione a aplicação atribuída anteriormente ao novo usuário

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/16.jpg "API WhatsApp Business")

3. Em **"Definir expiração"**, escolha **"Nunca"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/17.jpg "API WhatsApp Business")

4. Em **"Atribuir permissões"**, selecione as 3 últimas opções que iniciam com **"whatsapp_business_"**:
   - `whatsapp_business_manage_events`
   - `whatsapp_business_management`
   - `whatsapp_business_messaging`

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/18.jpg "API WhatsApp Business")

5. Clique em **"Gerar token"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/19.jpg "API WhatsApp Business")

**IMPORTANTE:** Copie o token gerado e guarde essa informação em um arquivo seguro (bloco de notas, Word, etc.).

---

## 5 - CONFIGURANDO O NÚMERO DE TELEFONE

1. Clique no botão azul **"Começar a usar a API"** (ou acesse pelo menu esquerdo: WhatsApp > Início rápido)

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/20.jpg "API WhatsApp Business")

2. Na opção **"De"**, clique onde aparece **"Número de teste"**
3. Clique em **"Adicionar telefone"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/21.jpg "API WhatsApp Business")

### Informações da Empresa

1. Informe os dados de sua empresa
2. Clique em **"Avançar"**

    ![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/22.jpg "API WhatsApp Business")

### Categoria e Fuso Horário

1. Em **"Categoria"**, escolha o segmento de atuação de sua empresa
2. Ajuste o **"Fuso horário"**
3. Clique em **"Avançar"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/23.jpg "API WhatsApp Business")

### Adicionando Número de Telefone

1. Informe o **número de telefone**
2. Escolha como quer receber a confirmação do Facebook: **SMS** ou **Ligação telefônica**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/24.jpg "API WhatsApp Business")

**⚠️ IMPORTANTE:** O número informado **não pode estar vinculado a nenhuma outra conta com o WhatsApp**. Se quiser utilizar um número nessa condição, terá de excluir a outra conta primeiro.

3. Informe o **código de verificação** recebido
4. Clique em **"Avançar"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/25.jpg "API WhatsApp Business")

---

## 6 - AGUARDANDO APROVAÇÃO

Agora é necessário esperar a aprovação e confirmação por parte da Meta. Quando seu número for aprovado, você receberá um email da empresa Meta confirmando sua efetivação.

### Códigos Importantes

Guarde o código de **"Identificação da conta do WhatsApp Business"** que aparecerá na tela. Este código é chamado de **"WABA"** e será utilizado posteriormente junto com o **token** gerado anteriormente.

O WABA está disponível no menu **WhatsApp > Configuração de API**.

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/26.jpg "API WhatsApp Business")

### Configurando Forma de Pagamento

Ainda nesta tela, você verá pendências envolvendo a "forma de pagamento" que ainda não foi selecionada.

1. Clique em **"Ir para o Gerenciador do WhatsApp"**
2. Clique em **"Adicionar uma forma de pagamento"**
3. Informe seus dados à Meta

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/27.jpg "API WhatsApp Business")

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/28.jpg "API WhatsApp Business")

Esse alerta de pendências desaparecerá após configurar a forma de pagamento.

### Verificando Status do Número

Para checar o status do seu número com a Meta:

1. Acesse sua conta business
2. Clique no menu em **"Todas as ferramentas"**
3. Clique em **"Gerenciador do WhatsApp"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/29.jpg "API WhatsApp Business")

4. No menu à esquerda, clique em **"Números de telefones"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/30.jpg "API WhatsApp Business")

---

## 7 - ATIVANDO NO SISTEMA VIP

Após receber a confirmação de ativação do número por parte da Meta, você estará apto para ativar a API do WhatsApp no sistema Vip.

### Requisitos

- Acesso com perfil **admin**

### Passo a Passo

1. No sistema Vip, navegue até **WhatsApp > Status de Conexão**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/31.jpg "API WhatsApp Business")

2. Informe o código **WABA** (obtido nas configurações do Meta)
3. Informe o **token** gerado anteriormente
4. Clique em **"Conectar"** para prosseguir
5. Será exibido o nome do seu portfólio empresarial e o número de telefone cadastrado

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/32.jpg "API WhatsApp Business")

6. Clique no nome do seu **"Portfólio empresarial"** para confirmar a utilização do mesmo

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/33.jpg "API WhatsApp Business")

### Recebendo Dados para Cadastro

O sistema Vip fornecerá duas informações para cadastrar no sistema da Meta:

- **Webhook**: URL de callback
- **Token**: Token de verificação

---

## 8 - CONFIGURANDO WEBHOOKS NO META

De volta às configurações do aplicativo:

1. Clique em **"WhatsApp"** > **"Configuração"**
2. Em **"URL de callback"**, informe o valor **"Webhook"** (fornecido pelo sistema Wip)
3. No campo **"Verificar token"**, insira o **Token** (fornecido pelo sistema Wip)
4. Clique em **"Verificar e salvar"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/34.jpg "API WhatsApp Business")

### Selecionando o Produto

1. Clique em **"Selecione o produto"**
2. Escolha a opção **"Whatsapp Business Account"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/35.jpg "API WhatsApp Business")

### Ativando Webhooks

Role a página e ative as seguintes opções:

- `message_template_quality_update`
- `message_template_status_update`
- `messages`

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/36.jpg "API WhatsApp Business")

### Salvando Configurações

1. Role a página para cima novamente
2. Informe novamente seu **token**
3. Clique em **"Verificar e salvar"**

![API WhatsApp Business](../../assets/img/suporte/whatsapp-business-api/37.jpg "API WhatsApp Business")

---

## Conclusão

Após realizar todas estas configurações, a integração com o **WhatsApp Business API** estará concluída e você já poderá utilizar o sistema **Wip da Vip Solutions** para enviar e receber mensagens via WhatsApp.

---

## Referências Importantes

- **WABA**: Código de Identificação da conta do WhatsApp Business
- **Token**: Código de acesso gerado para autenticação
- **Webhook**: URL de callback para receber notificações
- **Portfólio Empresarial**: Conta empresarial na Meta
