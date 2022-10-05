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
      contents: read
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
# TODO : Add issue spend
````