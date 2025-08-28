# My website!

Hi, you've found the source code for my website https://foxes.systems.

This makes use of the following projects:

- [bic](https://bic.sh): static site generation
    - [mo](https://github.com/tests-always-included/mo): templating
    - [md-to-html](https://github.com/moltenib/md-to-html): markdown to html coversion
    - [sqids](https://sqids.org): unique identifiers
- [Dracula theme](https://github.com/dracula/dracula-theme): color pallete
- [IBM Plex](https://www.ibm.com/plex/): Font family
- [Font Awesome](https://fontawesome.com/): Icon typeface
- [highlight.js](https://highlightjs.org/): Code syntax highlighting

## Building

I use a modified version of [bic](https://bic.sh/) to handle page templating,
markdown to html conversion and generating of rss feeds.

A build of the final static site can be done by running

```
$ ./bic.sh
```

The only depdenencies required are bash and coreutils.
