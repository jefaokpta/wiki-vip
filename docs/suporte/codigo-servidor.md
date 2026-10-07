# Configuração do código do servidor

Para cada novo servidor, o financeiro da VIP abre um chamado informando o código que deverá ser configurado. 

A equipe de DevOps realiza o ajuste diretamente no banco de dados do servidor.

O ajuste é feito na tabela `relcalls`, utilizada pelo relatório **Chamadas no período**. O código padrão é `099` e deve ser substituído pelo código informado no chamado. Por exemplo, se o financeiro informar `021`, use esse valor nas duas instruções abaixo:

```sql
ALTER TABLE relcalls
    CHANGE servidor_id servidor_id VARCHAR(3) NOT NULL DEFAULT '021';

UPDATE relcalls
SET servidor_id = '021';
```

Execute as instruções no banco de dados do servidor correspondente e substitua `021` pelo código recebido no chamado. Confirme o servidor e o código antes de executar: o `UPDATE` altera os registros existentes da tabela.

Após a execução, o histórico de chamadas passará a usar o novo código e as chamadas futuras serão registradas com esse código, definido como padrão da coluna.
