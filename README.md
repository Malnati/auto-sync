# auto-sync

Workflow reutilizável do GitHub Actions para manter branches de desenvolvimento sincronizados com seus ramos base em pull requests, automatizando merges e PRs de sincronização em fluxos como `development → staging → main`.

## Funcionalidades

- Detecta commits pendentes no branch base em relação ao branch de origem da PR.
- Para branches protegidos (`main`, `staging`, `development`), cria um branch de sincronização e abre uma PR automática.
- Para branches não protegidos, faz o merge direto do ramo base no ramo da PR.
- Comenta na PR original informando o que foi feito e quais arquivos ainda divergem do ramo base.

## Uso

No repositório de aplicação, crie um workflow que chame este workflow reutilizável:

```yaml
# .github/workflows/pr-flow.yml
name: PR Flow

on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  flow-check:
    uses: Malnati/flow-check/.github/workflows/flow-check.yml@v1

  auto-sync:
    needs: flow-check
    uses: Malnati/auto-sync/.github/workflows/auto-sync.yml@v1
