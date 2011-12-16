---
layout: default
title: README
---

Project-wide Documentation for Integrating Digital Papyrology
=============================================================

[This repository](https://github.com/papyri/documentation) will contain:

* System-level documentation about how project components are used and interact
* Documentation and scripts for generating documentation for project components

## Requirements for Generating Documentation

* Ruby
* Git
* Apache Maven
* `yard` gem (for RXSugar documentation)
* [Jekyll](https://github.com/mojombo/jekyll) (optional)

## Usage

Running `rake` will run the default task to fetch component repositories and build
their documentation.

Running `rake jekyll:static` on the `gh-pages` branch will generate the static site
which is the same as that served via GitHub Pages. This will generate a `_site`
directory suitable for copying to other webservers.

Documentation for Markdown pages (i.e. this `README`, system documentation under
`system_level`) should be updated on `master` then merged back to `gh-pages`,
but `gh-pages` should not be merged back to `master`. This is to keep the generated
documentation only under `gh-pages` and keep `master` relatively clean. The
`gh-pages` branch also has [YAML front matter](https://github.com/mojombo/jekyll/wiki/yaml-front-matter)
for Markdown pages, so that Jekyll can correctly convert them into HTML and the
indexing task has a title to assign to them. Not merging this back onto `master`
keeps the rendered Markdown on the GitHub repository view clean.

## System-Level Documentation

{% include system_level_index.md %}

## Generated Documentation

{% include generated_index.md %}
