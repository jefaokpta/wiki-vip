# Exemplos de HTML na Wiki

Esta página demonstra as duas formas de usar arquivos HTML na wiki.

## 1. Embed via iframe (diagramas pequenos)

Ideal para diagramas, fluxos simples ou widgets compactos.

<iframe src="diagramas/exemplo.html" width="100%" height="280" frameborder="0"></iframe>

Código usado:

```html
<iframe src="diagramas/exemplo.html" width="100%" height="280" frameborder="0"></iframe>
```

## 2. Link direto (dashboards completos)

Ideal para dashboards interativos ou páginas HTML complexas.

- [Abrir dashboard de exemplo](dashboards/exemplo.html)

## Onde colocar arquivos HTML

| Tipo | Pasta |
|------|-------|
| Diagramas embeddáveis | `docs/html/diagramas/` |
| Dashboards standalone | `docs/html/dashboards/` |

## Referência

Veja também a ficha [srv-nodered-prod](../dominios/integracoes/prod/servidores/srv-nodered-prod.md) que usa ambos os padrões.
