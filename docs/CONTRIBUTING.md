# Guia de Contribuição — Wiki VIP

Obrigado por contribuir com a wiki de infraestrutura. Este documento define as convenções para manter a documentação organizada e segura.

## Repositório público

<div class="alert-public">

Este repositório é **público**. Nunca inclua:

- Senhas, tokens ou API keys
- Chaves privadas SSH/TLS
- Credenciais de banco de dados
- Dados pessoais sensíveis

Use links para vault ou secret manager quando necessário referenciar credenciais.

</div>

## Estrutura de pastas

```
docs/
├── global/              # Transversal (redes, serviços compartilhados)
├── ambientes/           # Índices agregadores por Prod/Dev
├── dominios/            # Conteúdo real — ownership por time
│   └── {dominio}/
│       ├── prod/
│       └── dev/
└── html/                # Páginas HTML standalone
    ├── diagramas/
    └── dashboards/
```

## Convenções de naming

- Pastas e arquivos em **kebab-case**: `integracoes`, `srv-web-01.md`
- Fichas de servidor: `srv-{hostname}.md`
- README.md como índice de cada pasta

## Nova ficha de servidor

1. Copie `dominios/_template/prod/servidores/_exemplo-servidor.md`
2. Salve em `dominios/{dominio}/{ambiente}/servidores/srv-{hostname}.md`
3. Preencha os campos mínimos: hostname, IP, SO, função, responsável, links
4. Adicione link na página de camada em `ambientes/{prod|dev}/camada-{web|app|database}.md`
5. Adicione link no README do domínio/ambiente

## Novo domínio

1. Copie `dominios/_template/` para `dominios/{nome}/`
2. Preencha os READMEs
3. Adicione entrada em `_sidebar.md`
4. Opcional: registre owner em `.github/CODEOWNERS`

## Processo de PR

1. Crie uma branch a partir de `main`
2. Faça suas alterações seguindo as convenções acima
3. Abra um Pull Request descrevendo o que foi documentado
4. Solicite review do responsável pelo domínio
5. Após merge, o GitHub Pages publica automaticamente

## Dúvidas

Contato o Edson Ferino atraés do email edson@vipsolutions.com.br.
