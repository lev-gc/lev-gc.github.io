# GitHub Pages

[![Deploy GitHub Pages](https://github.com/lev-gc/lev-gc.github.io/actions/workflows/deploy-pages.yml/badge.svg?branch=source)](https://github.com/lev-gc/lev-gc.github.io/actions/workflows/deploy-pages.yml)

## About the project

The source files of my [Blog](https://lev-gc.github.io/).

Maybe some old articles.

## Get Started

After cloning the project:
- change directory to the root path of the project
- use Node.js `>= 20.19.0`
- use npm `>= 11`
- execute command `npm install`

For Hexo:
- clean project: `npm run clean` or `npm run c` or `hexo clean`
- compile project: `npm run build` or `npm run b` or `hexo generate` or `hexo g`
- start server on [localhost](http://localhost:4000/): `npm run server` or `npm run s`

## Deployment

- deployment is handled by GitHub Actions
- pushing to the `source` branch triggers a Pages deployment
- if you still use Google Search Console verification, add the `GOOGLE_VERIFICATION` repository secret
