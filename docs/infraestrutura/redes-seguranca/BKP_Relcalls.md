# Backup de relcalls

Este procedimento restaura relatórios de chamadas de um cliente que foi migrado de um servidor para outro, quando os números de empresa (`company_id`, ou control number) são diferentes.

## Exportar os dados do servidor antigo

1. Acesse via PMA o servidor que será desligado.
2. Execute no banco de dados a consulta abaixo, substituindo os valores entre aspas:

   ```sql
   UPDATE `relcalls`
   SET `company_id` = "numero_empresa_no_servidor"
   WHERE `calldate` > "data 3 meses atras"
     AND `company_id` = "numero_empresa_servidor_antigo";
   ```

3. Execute a consulta para obter os dados convertidos:

   ```sql
   SELECT `calldate`, `src`, `srcfinal`, `dstfinal`, `disposition`, `duration`,
          `billsec`, `accountcode`, `uniqueid`, `channel`, `trunk_id`, `price`,
          `userfield`, `hide`, `cidade`, `estado`, `company_id`, `servidor_id`
   FROM `relcalls`
   WHERE `calldate` > "data 3 meses atras"
     AND `company_id` = "numero_empresa_no_servidor";
   ```

4. Abaixo dos dados exibidos, clique em **Exportar**.
5. Na página seguinte, selecione:
   - **Método de exportação:** Personalizado
   - **Formato:** SQL
   - Em **Opções específicas do formato**, na última opção (**Database system or older MySQL server to maximize output compatibility with:**), selecione **dados**.
6. Clique em **Executar**.
7. Copie o arquivo `relcalls.sql` gerado para o servidor de destino.

## Importar os dados no servidor de destino

No servidor de destino, execute:

```sh
mysql -uroot -p vip < relcalls.sql
```

> **Atenção:** A importação pode gerar alto uso de recursos do servidor. Execute-a fora do horário de expediente, após criar um snapshot do servidor. Se ocorrer algum erro, restaure o snapshot.
