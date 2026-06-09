# Wiki VIP — Infraestrutura

Wiki de infraestrutura da empresa, construída com [Docsify.js](https://docsify.js.org/) e publicada via **GitHub Pages**.

## Acesso

Após configurar o GitHub Pages, a wiki estará disponível em:

```
https://{org-ou-usuario}.github.io/{nome-do-repo}/
```

## Desenvolvimento local


Abra `http://localhost:3000` no navegador.

Com Node.js:

```bash
npx serve .
```

## Publicação — GitHub Pages

Configure no repositório GitHub em **Settings → Pages**:

| Campo | Valor |
|-------|--------|
| Source | Deploy from a branch |
| Branch | `main` |
| Folder | `/ (root)` |

O `index.html` na raiz carrega o conteúdo de `docs/` via Docsify. O arquivo `.nojekyll` garante que arquivos com prefixo `_` (como `_sidebar.md`) sejam servidos corretamente.

### Estrutura do repositório

```
wiki-vip/
├── index.html          # Configuração Docsify + plugins
├── .nojekyll           # Necessário para GitHub Pages
├── README.md           # Este arquivo
├── CONTRIBUTING.md     # Guia para colaboradores
├── assets/img/         # Logo e imagens estáticas
└── docs/               # Conteúdo da wiki
    ├── README.md       # Home
    ├── _sidebar.md     # Navegação
    └── ...
```

## Plugins Docsify

| Plugin | Função |
|--------|--------|
| search | Busca full-text |
| pagination | Navegação anterior/próximo |
| copy-code | Copiar blocos de código |
| mermaid | Diagramas em Markdown |
| sidebar-collapse | Sidebar colapsável |
| tabs | Abas para comparar conteúdo |
| dark-switcher | Toggle claro/escuro no canto inferior direito (preferência salva em `localStorage`) |

## Contribuir

Leia o [Guia de Contribuição](docs/CONTRIBUTING.md) para convenções de estrutura, naming e processo de PR.

## Hierarquia da documentação

- **Global** — Redes, DNS, serviços compartilhados (transversal)
- **Ambientes** — Índices Prod e Dev por camada (Web, App, Database)
- **Domínios** — Ownership por time (Integrações, Monitoramento, CI/CD)

Conteúdo real de servidores vive em `docs/dominios/`. Os índices em `docs/ambientes/` apenas linkam para as fichas.
