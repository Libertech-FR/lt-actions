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
name: Update Time Spent On Issues

on:
  issue_comment:
    types: [created, edited]

jobs:
  get-comments:
    runs-on: ubuntu-latest
    outputs:
      comments: ${{ steps.generate-matrix.outputs.comments }}

    steps:
      #       - uses: actions/checkout@v2
      - name: Time spent control
        uses: Libertech-FR/lt-actions/issue-spend@main
        #         if: ${{ startsWith(github.event.comment.body, '!spend') }}
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
````