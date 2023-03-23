#Lt Actions
## Docker image
```yml
name: Docker Image CI

on:
  release:
    types: [published]

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      packages: write

    steps:
      - name: Checkout the code
        uses: actions/checkout@v2

      - name: Build docker Nuxt
        uses: Libertech-FR/lt-actions/docker-image@main
        with:
          repository: ${{ github.repository }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
```
## Issue spend
````yml
name: Update Time Spent On Issues

on:
  issue_comment:
    types: [created, edited]

jobs:
  get-comments:
    if: ${{ startsWith(github.event.comment.body, '!spend') }}
    runs-on: ubuntu-latest
    outputs:
      comments: ${{ steps.generate-matrix.outputs.comments }}

    steps:
      - name: Time spent control
        uses: Libertech-FR/lt-actions/issue-spend@main
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
````

## Publish release
Créer un fichier release.yml dans le dossier .github/workflows/ :

```yml
name: Release

on:
  workflow_dispatch:
    inputs:
      version_increment:
        description: 'La version a incrémenter (major, minor, patch)'
        required: true
        default: 'patch'
        type: choice
        options:
        - 'major'
        - 'minor'
        - 'patch'
      build_docker_image:
        description: "Construire l'image docker ?"
        required: true
        default: true
        type: boolean
      latest:
        description: "Tagger l'image docker avec le tag 'latest' ?"
        required: true
        default: true
        type: boolean

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write

    steps:
      - name: Build docker
        uses: Libertech-FR/lt-actions/release@main
        with:
          version_increment: ${{ github.event.inputs.version_increment }}
          build_docker_image: ${{ github.event.inputs.build_docker_image }}
          latest: ${{ github.event.inputs.latest }}
          repository: ${{ github.repository }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
          github_token: ${{ secrets.GITHUB_TOKEN }}
          # Optional parameters, thoses are default values :
          registry: 'ghcr.io'
          context: .
          build-args: ''
```
Ajouter ce badge dans le README.md du projet pour afficher le dernier tag de release :

```md
[![Release](https://github.com/<repo_owner>/<repo_name>/actions/workflows/release.yml/badge.svg)](https://github.com/<repo_owner>/<repo_name>/actions/workflows/release.yml?event=workflow_dispatch)
```