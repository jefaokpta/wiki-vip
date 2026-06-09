# Template de Domínio

Copie esta pasta para criar um novo domínio de negócio.

## Como usar

1. Copie `dominios/_template/` para `dominios/{seu-dominio}/`
2. Renomeie e preencha os READMEs
3. Adicione entrada em `docs/_sidebar.md`
4. Registre o owner em `.github/CODEOWNERS` (opcional)
5. Crie fichas em `{ambiente}/servidores/`
6. Atualize os índices em `ambientes/prod/` e `ambientes/dev/`

## Estrutura

```
dominios/{nome}/
├── README.md           # Visão geral do domínio
├── prod/
│   ├── README.md
│   └── servidores/
│       └── srv-*.md
└── dev/
    ├── README.md
    └── servidores/
        └── srv-*.md
```

## Responsável

_@usuario-github — preencher_
