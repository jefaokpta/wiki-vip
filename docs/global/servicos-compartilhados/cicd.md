# CI/CD

_Pipelines, runners e processos de deploy._

## Ferramentas

| Ferramenta | URL | Responsável |
|------------|-----|-------------|
| _GitHub Actions / Jenkins / etc._ | _—_ | _—_ |

## Fluxo de deploy

```mermaid
flowchart LR
    PR[Pull Request] --> CI[Pipeline CI]
    CI --> Review[Code Review]
    Review --> Merge[Merge main]
    Merge --> CD[Deploy]
```

## Links

- [Domínio CI/CD](../../dominios/cicd/README.md)
