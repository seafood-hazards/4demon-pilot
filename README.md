# 4Demon Pilot Database

[![DOI](https://zenodo.org/badge/1231115108.svg)](https://doi.org/10.5281/zenodo.20056924)

This repository contains the source Quarto Markdown documents for the [Four Decades of Belgian Marine Monitoring (4Demon)](https://www.vliz.be/projects/4demon/index.htm) website.

## License
This project is licensed under the [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) license.

## Data source
The original data used in this project is available on the [4Demon](https://www.vliz.be/projects/4demon/index.htm) website.

## Reproducibility

This project is designed to be fully reproducible. All required R package versions are recorded using [renv](https://rstudio.github.io/renv/), ensuring a consistent computational environment.

To reproduce the analysis and website locally:

 1. Clone the repository
 2. Open the project in R
 3. Restore the package environment:

```R
renv::restore()
```

 4. Render the website:

Use the ``Render Website`` option in RStudio. 

> [!Note]
> The website deployed on GitHub Pages is automatically built using the same workflow and environment configuration.

## Branching & Deployment

This repository follows [Gitflow](https://nvie.com/posts/a-successful-git-branching-model/): work happens on `feature/*` branches off `develop`, merged back into `develop` without a pull request. `main` reflects the published site — merging/pushing to `main` triggers `.github/workflows/publish.yml`, which renders the Quarto site and deploys it to GitHub Pages. Pushes to `develop` do not deploy.
