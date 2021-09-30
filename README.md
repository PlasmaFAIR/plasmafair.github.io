PlasmaFAIR website
==================

This is the repo for the PlasmaFAIR website. PlasmaFAIR is a new
community network of RSEs working in plasma science. Our goal is to
improve the quality and sustainability of plasma research software.

## Building the website

This website is built using Jekyll and Github Pages. The easiest way
to build this locally is:

```bash
$ gem install --user-install bundler jekyll
$ # You might need to make sure bundle is in your PATH: gem should tell you where it's put it
$ bundle config set path 'vendor/bundle'
$ bundle install vendor/bundle
$ bundle exec jekyll serve
```
