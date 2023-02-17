# Reusable Workflows

Conjunto de workflows reutilizáveis, como proposto nos vídeos:

- [GitHub Actions Reusable Workflows FULL TUTORIAL with Examples: Templates on Steroids](https://www.youtube.com/watch?v=lRypYtmbKMs)
- [GitHub Actions - Calling Reusable Workflows](https://www.youtube.com/watch?v=2dxmvDL1gP8)

Quando um workflow reutilizável for criado sua documentação deverá ser adicionada abaixo, devendo os `secrets` serem bem entendidos, evitando erros como ["invalid secret, is not defined in the referenced workflow"](https://github.com/orgs/community/discussions/26749).

## Publicação / Atualização de um Dataset Template

[O workflow reutilizável](https://github.com/transparencia-mg/reusable-workflows/blob/main/.github/workflows/publicacao_atualizacao_dataset_template.yml) foi criado para automatizar o processo de publicação ou atualização de um [Dataset Template](https://github.com/transparencia-mg/dataset-template).

As configurações abaixo deverão ser criadas no repositório que irá chamar este workflow reutilizável:

- GitHub secrets:
  - CKAN_HOST
  - CKAN_KEY_<USUARIO_GITHUB>

Crie o arquivo `.github/workflow/publicacao_atualizacao_dataset_template.yml` com o conteúdo a seguir:

```
# This uses a reusable workflow
name: Workflow reutilizável para publicação / atualização de Dataset Template

on:
  push:
    branches:
        - main
    paths:
      - 'datapackage.yaml'
      - 'schemas/*'
      - 'README.md'
      - 'CHANGELOG.md'
      - 'CONTRIBUTING.md'
      - 'upload/*'

jobs:
  do-it:
    steps:
      - id: string
        uses: ASzc/change-string-case-action@v1
        with:
          string: ${{ github.actor }}
      uses: transparencia-mg/reusable-workflows/.github/workflows/publicacao_atualizacao_dataset_template.yml@main
      secrets:
        CKAN_HOST: ${{ secrets.CKAN_HOST }}
        CKAN_KEY: ${{ secrets[format('CKAN_KEY_{0}', steps.string.outputs.uppercase)] }}
```