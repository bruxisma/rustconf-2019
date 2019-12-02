# Presentation from Rustconf 2019

These are the slides from my talk at Rustconf 2019. It uses Hugo with Reveal JS.

## Using

Until I make either the source, or the distribution their own branch, and have them online, you'll need
to [build](#building) this yourself. It's output should be usable in any modern web-browser, such as 
recent Firefox, Chrome, Edge or Safari web-browsers.

Alternatively you can watch the YouTube video https://www.youtube.com/watch?v=YZomx3Jt4Xs

## Building

Once Hugo is installed (see above); issuing the `hugo server` or simply `hugo` commands will help 
you to use this. The theme is setup as a git submodule, so you'll have to ensure that is initialised 
and present in the `themes/reveal-hugo` directory.

### Zip Archive

If downloading from a zip archive rather than a git clone, use the following to build the site from
the unpacked folder as your present working directory

```
rm -rf themes/reveal-hugo
git submodule add git@github.com:dzello/reveal-hugo.git themes/reveal-hugo
hugo
```

### Git Clone

```
git clone --recurse-submodules https://github.com/slurps-mad-rips/rustconf-2019.git rustconf-slurps-mad-rips-2019
cd rustconf-slurps-mad-rips-2019
hugo
```

## Tooling

This repo uses two tools primarily

- [Git](https://git-scm.com/)
- [Hugo](https://gohugo.io/)
- [RevealJS](https://revealjs.com/)
- [reveal-hugo theme](https://github.com/dzello/reveal-hugo)

### Hugo

If you're not familiar with Hugo, it's a tool, which this uses to generate the slides from markdown content.

The following links are the best place to get help with Hugo

- https://gohugo.io/getting-started/installing/
- https://gohugo.io/getting-started/usage/

This has been tested building with Hugo 0.55.6

### Git

The recommended version of git to use is 2.13 or later, because of the clone with submodules feature. 
You can check your version using `git --version` command.

You can use GitHub download to avoid using the git dependency, but will then need an unarchiver for the 
format you download.
