# Example of deploy using gh-pages and gh actions

Visit on: https://divorcedlance.github.io/gh-actions-deploy-test/

Based on: https://www.youtube.com/watch?v=XhoWXhyuW_I

## Steps:

###  Setup base in vite.config

```js
base: "/[REPO_NAME]/"
```

### Create ./github/workflows/deploy.yml and add the code bellow

```yml
name: Deploy

on:
  push:
    branches:
      - main

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v2

      - name: Setup Node
        uses: actions/setup-node@v1
        with:
          node-version: 16

      - name: Install dependencies
        uses: bahmutov/npm-install@v1

      - name: Build project
        run: npm run build

      - name: Upload production-ready build files
        uses: actions/upload-artifact@v2
        with:
          name: production-files
          path: ./dist

  deploy:
    name: Deploy
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
      - name: Download artifact
        uses: actions/download-artifact@v2
        with:
          name: production-files
          path: ./dist

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

### Active Workflow (GitHub)

```
Config > Actions > General > Workflow permissions > Read and Write permissions 
```

```
Actions > failed deploy > re-run-job failed jobs 
```

```
Pages > gh-pages > save
```
