Project-wide Documentation for Integrating Digital Papyrology
=============================================================

This repository will contain:

* System-level documentation about how project components are used and interact
* Documentation and scripts for generating documentation for project components

## Requirements for Generating Documentation

* Ruby
* Git
* Apache Maven
* [Jekyll](https://github.com/mojombo/jekyll) (optional)

## Usage

Running `rake` will run the default task to fetch component repositories and build
their documentation.

Running `rake jekyll:static` on the `gh-pages` branch will generate the static site
which is the same as that served via GitHub Pages. This will generate a `_site`
directory suitable for copying to other webservers.
