# Chirpy Starter

[![Gem Version](https://img.shields.io/gem/v/jekyll-theme-chirpy)][gem]&nbsp;
[![GitHub license](https://img.shields.io/github/license/cotes2020/chirpy-starter.svg?color=blue)][mit]

A minimal, ready-to-use template for creating a blog with the [**Chirpy**][chirpy] Jekyll theme. Get up and running in minutes with all critical files pre-configured.

## Why This Starter Exists

When installing Chirpy through [RubyGems.org][gem], Jekyll can only read a subset of theme files (`_data`, `_layouts`, `_includes`, `_sass`, `assets`) and limited `_config.yml` options from the gem. As a result, users cannot enjoy the full out-of-the-box experience that Chirpy offers.

To unlock all features, the following files must be present in your Jekyll site:

```shell
.
├── _config.yml
├── _plugins
├── _tabs
└── index.html
```

This starter bundles those files from the latest **Chirpy** release along with a [CD][CD] workflow, so you can start writing immediately.

## Usage

Check out the [theme's docs](https://github.com/cotes2020/jekyll-theme-chirpy/wiki).

## Contributing

This repository is automatically updated with new releases from the theme repository. If you encounter any issues or want to contribute to its improvement, please visit the [theme repository][chirpy] to provide feedback.

## License

This work is published under [MIT][mit] License.

[gem]: https://rubygems.org/gems/jekyll-theme-chirpy
[chirpy]: https://github.com/cotes2020/jekyll-theme-chirpy/
[CD]: https://en.wikipedia.org/wiki/Continuous_deployment
[mit]: https://github.com/cotes2020/chirpy-starter/blob/master/LICENSE


## Tung Anh's note
Instruction taken from this video: https://www.youtube.com/watch?v=m1RYsmOMPLs
TLDR:
### Download Ruby 
Download Ruby and follow ALL instructions here: https://jekyllrb.com/docs/installation/windows/
When I run `jekyll -v`, the output is `4.4.1`
Note: chirpy does NOT support ruby 4.0. Do not download the latest Ruby version 3.3.12

### Install dependency
Run `bundle` in terminal in your project.
### Run server
Run `bundle exec jekyll s` in terminal in your project. The terminal will outputs `http://127.0.0.1:4000/`: this is your local blog server running.
Deploying this on Github Pages will host your blogs on Github.
### Edit & Modification
Open file `_config.yaml`, set parameter to your Github Page URL (`https://yourGithubUsername.github.io)