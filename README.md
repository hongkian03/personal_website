# Personal Website

A fast, static personal website for Kian Hong Tan: developer portfolio, research-facing profile, project archive, experience narrative, and opinion blog.

## Status

The initial Hugo site is scaffolded with placeholder copy that is ready to be replaced by reviewed resume, project, experience, and writing material.

## Product direction

The site should work equally well as:

- a software-development portfolio;
- a profile for PhD applications and research interests; and
- a durable home for public writing.

The site is designed to work equally well as a software-development portfolio, research-facing profile, and durable home for public writing.

## Planned stack

- [Hugo](https://gohugo.io/) extended edition
- [hugo-flex](https://github.com/ldeso/hugo-flex) v1.6.2, included as a Git submodule and customized through this project
- GitHub Pages, deployed by GitHub Actions
- Markdown content, with assets stored in the repository

## Local development

```sh
hugo server -D
hugo --gc --minify
```

The GitHub Pages workflow builds with its deployment URL, so it supports either a user site or a project site without committing generated output.

## Next milestone

Replace the initial placeholder content with reviewed resume/profile material, selected project case studies, and the first writing pieces. Configure GitHub Pages to use GitHub Actions once the repository is pushed to GitHub.
