# Guia de Contribuição — Wiki VIP

Obrigado por contribuir com a wiki de infraestrutura. Este documento define as convenções para manter a documentação organizada e segura.

## Repositório público

Este repositório é **público**. Nunca inclua:

- Senhas, tokens ou API keys
- Chaves privadas SSH/TLS
- Credenciais de banco de dados
- Dados pessoais sensíveis

Use links para vault ou secret manager quando necessário referenciar credenciais.

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

1. Copie `docs/dominios/_template/prod/servidores/_exemplo-servidor.md`
2. Salve em `docs/dominios/{dominio}/{ambiente}/servidores/srv-{hostname}.md`
3. Preencha os campos mínimos: hostname, IP, SO, função, responsável, links
4. Adicione link na página de camada em `docs/ambientes/{prod|dev}/camada-{web|app|database}.md`
5. Adicione link no README do domínio/ambiente

## Novo domínio

1. Copie `docs/dominios/_template/` para `docs/dominios/{nome}/`
2. Preencha os READMEs
3. Adicione entrada em `docs/_sidebar.md`
4. Opcional: registre owner em `.github/CODEOWNERS`

## Arquivos HTML

| Tipo | Pasta | Uso |
|------|-------|-----|
| Diagramas pequenos | `docs/html/diagramas/` | Embed via `<iframe>` em Markdown |
| Dashboards completos | `docs/html/dashboards/` | Link direto na sidebar ou ficha |

Veja [exemplo-html.md](docs/html/exemplo-html.md) para referência.

## Processo de PR

1. Crie uma branch a partir de `main`
2. Faça suas alterações seguindo as convenções acima
3. Abra um Pull Request descrevendo o que foi documentado
4. Solicite review do responsável pelo domínio
5. Após merge, o GitHub Pages publica automaticamente

## Checklist do autor

- [ ] Nenhuma credencial ou dado sensível no conteúdo
- [ ] Ficha segue o template mínimo
- [ ] Links adicionados nos índices de ambiente (se nova ficha)
- [ ] Sidebar atualizada (se novo domínio ou seção)
- [ ] Caminhos relativos corretos nos links Markdown

## Logo e assets

Coloque imagens estáticas em `assets/img/` na raiz do repositório.

Referência no Docsify (`index.html`):

```javascript
logo: '/assets/img/logo.png'
```

## Dúvidas

Abra uma issue no repositório ou contate @equipe-infra.
