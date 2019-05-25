# Kayzor docs

## Installation instructions

Clone this repo **recursively**:
```sh
git clone --recursive https://github.com/KayzorPlatform/docs
```
Change the working directory to the repo:
```sh
cd docs
```
Run `hugo` to build the project:
```sh
hugo -D --baseUrl=http://docs.kayzor.com/
```

The built project it's in the `public/` directory.


### Why does it have to be recursively?
Because the theme it uses is a
[fork](https://github.com/dreamtigers/dot-hugo-documentation-theme) of
[dot-hugo-documentation-theme](https://github.com/themefisher/dot-hugo-documentation-theme/)
and it's added as a git submodule.


## Dependencies

This project depends on the `hugo` SSG, which you can get (in Debian based
distros) with:

```sh
sudo apt-get install hugo
```
